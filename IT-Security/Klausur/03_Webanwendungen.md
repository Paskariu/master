# IT-Sicherheit 2 – Sicherheit von Web-Anwendungen

> **Foliensatz:** `2-webanwendungen_v11f.pdf`  
> **Klausurfokus:** Ablauf von Webanwendungen, Sessions/Cookies, Tracking, JavaScript-Sicherheit, SOP/CORS, OWASP Top 10 2025, XSS/SQL Injection/CSRF, CSP und Security Header.

---

# 1. Grundlagen von Web-Anwendungen

## 1.1 Ablauf eines Webseitenaufrufs

1. Browser löst den Hostnamen per **DNS** auf.
2. Browser sendet einen **HTTP-Request**.
3. Webserver verarbeitet die Anfrage.
4. Server erzeugt häufig dynamisch HTML, z. B. mit PHP, ASP.NET, Python oder node.js.
5. Server sendet einen **HTTP-Response**.
6. Browser lädt/verarbeitet HTML, CSS, JavaScript, Bilder und weitere Ressourcen und zeigt die Seite an.

Typische Architektur: **Multi-Tier-Architektur**

```text
Browser -> Webserver -> Application Server -> Datenbank
```

Angriffe können jede Schicht treffen:

| Schicht | Typische Probleme |
|---|---|
| Browser / Nutzer | Phishing, Webserver-Spoofing, XSS, CSRF |
| Netzwerk | Sniffing, Man-in-the-Middle, DNS Cache Poisoning, Session Hijacking |
| Web-/Application Server | Eingabefehler, Authentifizierungs- und Autorisierungsfehler, Command Injection |
| Datenbank | SQL Injection |

## 1.2 HTTP-Eigenschaften und Risiken

HTTP ist von sich aus nicht sicher:
- keine Verschlüsselung,
- keine Integritätssicherung,
- keine Serverauthentisierung.

Diese Eigenschaften entstehen erst mit **TLS/SSL**. Ohne TLS werden Sniffing und Maskierung erheblich erleichtert.

Privatsphäre-Risiken bei HTTP/Webnutzung:
- `Referer`-Header kann zuvor besuchte URL inkl. Pfad/Parametern preisgeben,
- Cookies ermöglichen Tracking,
- Browser-Fingerprinting erlaubt Wiedererkennung über Merkmalskombinationen.

## 1.3 Spoofing im Web

| Angriff | Bedeutung |
|---|---|
| **Typejacking** | Minimale Änderung eines Domainnamens, die kaum auffällt, z. B. `paypai` oder `paypa1` statt `paypal`. |
| **Address-Bar Spoofing** | Lange Domainnamen täuschen Nutzer, etwa `www.dhbw-stuttgart.de.malicious.hacker.site.com`. Der echte Host ist rechts: `hacker.site.com`. |
| Nachbau von Sicherheitsanzeigen | Gefälschte Browserdialoge oder Schlösser simulieren Sicherheit. |
| Kein Trusted Path | Nutzer kann kaum sicher unterscheiden, ob ein Browserdialog echt oder Teil einer Seite ist; Fullscreen-APIs können dies begünstigen. |

## 1.4 HTML5 und lokaler Speicher

HTML5 ermöglicht umfangreiche Anwendungen und Offline-Funktionen. Risiken:
- **Web Storage** und **IndexedDB** erweitern lokalen Speicher,
- persistente Speicherung kann Tracking erleichtern,
- persistenter Schadcode oder rechtswidrige Inhalte können abgelegt werden.

---

# 2. Session-Verwaltung und Cookies

## 2.1 Warum Sessions nötig sind

HTTP ist **zustandslos**. Jeder Request wird isoliert behandelt; ohne Zusatzmechanismus kann der Server mehrere Requests nicht zuverlässig einem Nutzer oder Vorgang zuordnen.

Lösungen:
- Cookies
- URL-Rewriting
- Hidden Form Fields

In der Praxis sind Cookies der Standard.

## 2.2 Funktionsprinzip einer Session mit Cookie

1. Beim ersten Abruf erzeugt der Server eine eindeutige Session-ID.
2. Server liefert sie per `Set-Cookie` im HTTP-Response aus.
3. Browser speichert das Cookie.
4. Bei späteren Requests an passende Domain/Pfad schickt Browser das Cookie automatisch mit.
5. Server ordnet die ID einer serverseitigen Session zu.

```text
Server -> Client:
Set-Cookie: session=13YW...K2; Path=/; Secure; HttpOnly

Client -> Server:
Cookie: session=13YW...K2
```

**Wichtig:** Das Cookie enthält häufig nur einen Identifier. Die eigentlichen Session-Daten liegen serverseitig.

## 2.3 Aufbau und Identität eines Cookies

Grundform:

```text
Set-Cookie: <Name>=<Value>; <Attribut>; <Attribut>; ...
```

Ein Cookie wird durch die Kombination identifiziert:

```text
(Name, Domain, Path)
```

Daher sind beispielsweise verschieden:

```text
(JSESSIONID, banking.bw-bank.de, /)
(JSESSIONID, bw-bank.de, /)
```

## 2.4 Cookie-Attribute

| Attribut | Bedeutung |
|---|---|
| `Domain` | Legt fest, für welche Domain(s) das Cookie bei Requests gesendet wird. |
| `Path` | Legt fest, für welche URL-Pfade das Cookie gesendet wird. |
| `Expires` | Absolutes Ablaufdatum eines persistenten Cookies. |
| `Max-Age` | Ablaufzeit in Sekunden. `Max-Age=0` löscht das Cookie. |
| `Secure` | Browser sendet Cookie nur über HTTPS. |
| `HttpOnly` | JavaScript darf Cookie nicht über `document.cookie` auslesen. |
| `SameSite` | Beschränkt Cookie-Übermittlung bei Cross-Site-Requests; wichtig gegen CSRF. |

### Session Cookie vs. Persistent Cookie

| Typ | Verhalten |
|---|---|
| **Session Cookie** | Kein `Expires`/`Max-Age`; wird typischerweise beim Schließen des Browsers entfernt. |
| **Persistent Cookie** | Hat `Expires` oder `Max-Age`; bleibt bis zum Ablauf gespeichert. |

## 2.5 Domain-Regeln

Ein Server darf nur Cookies für seine eigene Domain oder zulässige übergeordnete Suffixe setzen.

Beispiel: Cookie wird von `foo.example.com` gesetzt.

| `Domain`-Wert | Zulässig? | Geltung |
|---|---:|---|
| nicht gesetzt | ja | **Host-Only Cookie**: nur `foo.example.com` |
| `foo.example.com` | ja | `foo.example.com` und Subdomains |
| `example.com` | ja | `example.com` und Subdomains |
| `bar.foo.example.com` | nein | spezieller als der setzende Host |
| `.com` | nein | Top-Level-Domain |
| `web.de` | nein | fremde Domain |

Notation mit und ohne führenden Punkt ist gleichwertig:

```text
Domain=.example.com == Domain=example.com
```

## 2.6 Path-Regeln

Der Browser sendet ein Cookie, wenn der Cookie-Pfad Präfix des angeforderten URL-Pfads ist.

Beispiel: Cookie wird auf `foo.example.com/my/a/index.html` gesetzt.

| `Path` | Gültig für |
|---|---|
| nicht gesetzt | `/my/a/` und Unterpfade |
| `/my/` | `/my/` und Unterpfade |
| `/other/` | `/other/` und Unterpfade |

## 2.7 Wann sendet der Browser Cookies?

Ein Cookie wird gesendet, wenn:
1. `Domain` zum Host der URL passt  
   - bei Host-Only: exakt identisch  
   - sonst: Cookie-Domain ist Suffix des Hosts
2. `Path` Präfix des URL-Pfads ist.
3. Bei `Secure`: Request nutzt HTTPS.
4. Bei `SameSite`: zusätzlich die SameSite-Regel erfüllt ist.

**Wichtig:** Reihenfolge der übertragenen Cookies ist nicht verlässlich.

## 2.8 Cookies und JavaScript

JavaScript kann Cookies setzen, lesen und löschen:

```javascript
document.cookie = "ID=Alice; expires=...";
document.cookie = "ID=Alice; max-age=0";
```

`document.cookie` liefert nur Name-Value-Paare. Attribute werden nicht ausgelesen.

`HttpOnly` schützt gegen JavaScript-Auslesen, aber nicht gegen automatisches Mitsenden bei Requests.

---

# 3. Cookie-Sicherheitsprobleme

## 3.1 Client kann Cookie-Werte manipulieren

Der Server erhält nur das Name-Value-Paar. Er weiß nicht:
- welche ursprünglichen Attribute gesetzt waren,
- welche Domain das Cookie gesetzt hat,
- ob der Client den Wert verändert hat.

Folge: Sicherheitsrelevante Werte dürfen nie allein im Cookie als vertrauenswürdig behandelt werden.

Beispiel:

```text
login.dhbw.de setzt:
id=Alice; Domain=dhbw.de; Path=/

evil.dhbw.de setzt später:
id=Bob; Domain=dhbw.de; Path=/

moodle.dhbw.de erhält:
id=Bob
```

Dann kann eine Aktion fälschlich Bob zugerechnet werden.

## 3.2 Cookie-Manipulation

Cookies und Hidden Form Fields können mit Browser-Erweiterungen oder Request-Manipulation verändert werden.

Klassischer Fehler:

```text
price=19.99 -> price=0.01
```

**Gegenmaßnahme:** Preise, Berechtigungen, Rollen, Account-IDs und sonstige sicherheitskritische Werte immer serverseitig prüfen; niemals Clientdaten blind vertrauen.

## 3.3 Cookie-Diebstahl und Cookie-Missbrauch

| Angriff      | Idee                                                                                     |
| ------------ | ---------------------------------------------------------------------------------------- |
| **XSS**      | Schadcode liest Cookie aus und sendet ihn an Angreifer. `HttpOnly` erschwert dies.       |
| **CSRF**     | Browser sendet Session-Cookie automatisch mit; Angreifer missbraucht bestehende Sitzung. |
| **Tracking** | Persistente/Third-Party Cookies verbinden Aktivitäten über Seiten hinweg.                |

---

# 4. Tracking und Browser-Fingerprinting

## 4.1 Third-Party Cookies

Third-Party Cookies stammen von eingebundenen Drittanbietern, etwa durch:
- Anzeigen,
- JavaScript,
- iFrames,
- Tracking-Pixel.

Dadurch kann ein Drittanbieter Nutzer über verschiedene Websites wiedererkennen.

## 4.2 Browser Fingerprinting

Wiedererkennung ist auch nach Cookie-Löschung möglich.

Mögliche Merkmale:
- Browserversion
- Sprache
- installierte Fonts
- Bildschirmgröße und Farbtiefe
- HTTP-Header
- Zeitzone
- Browser-Plugins
- Rendering-Eigenschaften

### Canvas Fingerprinting
Canvas ist ein HTML5-Feature zum Zeichnen von Grafiken. Das Rendering kann je nach System, Browser, GPU, Fonts und Treibern leicht variieren. Das Ergebnis wird als Fingerprint genutzt.

### WebGL Fingerprinting
WebGL kann Hardware-/Renderinginformationen preisgeben und sogar browserübergreifende Fingerprints des Betriebssystems ermöglichen.

### ETag-Tracking
`ETag` ist eigentlich ein Caching-Mechanismus. Ein individueller ETag kann missbraucht werden, um Nutzer wiederzuerkennen.

## 4.3 Ende der Third-Party Cookies und Topics API

Die Folien behandeln die schrittweise Einschränkung von Third-Party Cookies:
- Safari blockiert sie seit Jahren.
- Chrome testete einen Ausstieg, zog eine vollständige separate Zustimmungslösung 2025 aber zurück.
- Der Wegfall bedroht viele Werbemodelle.

### Topics API

Grundidee:
1. Browser analysiert Browserverlauf.
2. Browser ordnet Interessen einer Taxonomie zu.
3. Für jede Woche werden einige Topics ermittelt.
4. Websites/AdTech erhalten begrenzt Topics für personalisierte Werbung.

**Wichtig:** Topics reduziert direkte Cross-Site-Tracking-Möglichkeiten, kann aber durch Kombination verschiedener Topics trotzdem Informationen über Nutzer offenbaren.

---

# 5. JavaScript-Sicherheit und Same Origin Policy

## 5.1 JavaScript-Sandbox

JavaScript wird im Browser in einer Sandbox ausgeführt:
- kein direkter Dateisystemzugriff,
- nur eingeschränkte Browser-APIs,
- Schutz sensibler Funktionen wie Verlauf oder Zwischenablage,
- Zugriff auf andere Websites wird zentral durch die **Same Origin Policy (SOP)** beschränkt.

## 5.2 Same Origin Policy
Die SOP untersagt JavaScript den lesenden Zugriff auf Objekte anderer Origins.

Eine **Origin** besteht aus:

```text
Protokoll + Hostname + Port
```

Nicht relevant ist die IP-Adresse.

Beispiele ausgehend von:

```text
http://www.example.com/dir/page.html
```

| Ziel | Gleiche Origin? | Grund |
|---|---:|---|
| `http://www.example.com/dir/page2.html` | ja | gleiches Schema, Host, Port |
| `http://www.example.com:80/dir/other.html` | ja | Port 80 ist HTTP-Standardport |
| `http://www.example.com:81/dir/other.html` | nein | anderer Port |
| `https://www.example.com/dir/other.html` | nein | anderes Protokoll |
| `http://en.example.com/dir/other.html` | nein | anderer Host |
| `http://example.com/dir/other.html` | nein | anderer Host |
| `data:image/...` | nein | anderes Schema |

## 5.3 Wichtige SOP-Grenzen

### Drittanbieter-Skripte

```html
<script src="https://fremdeseite.de/script.js"></script>
```

Das eingebundene Script läuft mit der Origin der einbindenden Seite. Es erhält daher Zugriff wie eigener Code.

Folge: Drittanbieter-JavaScript ist hochvertrauenswürdig und kann weitere Scripts nachladen.

### `document.domain`

Historisch konnten Subdomains auf eine gemeinsame Oberdomain gesetzt werden:

```javascript
document.domain = "example.com";
```

Dann können etwa `foo.example.com` und `bar.example.com` kommunizieren.

Risiko: Auch `evil.example.com` könnte beteiligt sein. Der Mechanismus ist daher problematisch und sollte vermieden werden.

### SOP verhindert nicht jeden Cross-Origin-Request

SOP verhindert in vielen Fällen das **Lesen** fremder Responses. Sie verhindert aber nicht generell das **Senden** von Requests an andere Origins. Genau das ist eine Grundlage für CSRF.

---

# 6. Subresource Integrity (SRI)

Websites binden häufig Drittressourcen ein:
- JavaScript-Bibliotheken,
- Fonts,
- Stylesheets,
- CDN-Inhalte.

**SRI** verhindert, dass eingebundene Ressourcen unbemerkt verändert oder ausgetauscht werden.

Beispiel:

```html
<script
  src="https://cdn.example.org/lib.min.js"
  integrity="sha512-..."
></script>
```

Der Browser lädt die Datei nur, wenn ihr Hash dem erwarteten Wert entspricht.

**Schutzziel:** Integrität von eingebundenem Drittcode.

Grenze: SRI schützt nicht gegen einen bewusst aktualisierten, aber bösartigen Hashwert im eigenen Deployment.

---

# 7. AJAX, Fetch und DNS Rebinding

## 7.1 AJAX / XMLHttpRequest / Fetch

AJAX = **Asynchronous JavaScript and XML**.

Zweck:
- Seite ohne komplettes Neuladen aktualisieren,
- Daten asynchron mit Server austauschen.

Älterer Mechanismus: `XMLHttpRequest` (XHR).  
Moderner Mechanismus: `fetch()`.

Sicherheitsrelevanz: Diese APIs unterliegen SOP und CORS.

## 7.2 DNS Rebinding
DNS Rebinding versucht, die SOP durch wechselnde DNS-Auflösungen zu umgehen.

Ablauf bei Time-Varying DNS:
1. Opfer lädt JavaScript von `attacker.com`.
2. DNS antwortet zunächst mit Angreifer-IP.
3. Antwort hat kurze TTL, damit Browser schnell erneut auflöst.
4. Angreifer-DNS ändert Zuordnung: `attacker.com -> interne Opfer-IP`.
5. JavaScript bleibt für Browser unter `attacker.com` und kann nun Requests an interne Ziel-IP ausführen/lesen.

Mögliche Folgen:
- Zugriff auf interne Webinterfaces, Drucker oder Router,
- Scans interner Netze,
- Umgehung von Firewalls,
- Umgehung IP-basierter Authentisierung,
- Missbrauch der Opfer-IP für Spam oder Click Fraud.

Gegenmaßnahmen:
- **DNS Pinning:** Browser merkt sich IP für gewisse Zeit.
- extrem kurze TTLs ablehnen bzw. absichern,
- Firewalls dürfen keine externen DNS-Antworten für interne IP-Bereiche akzeptieren.

---

# 8. CORS – kontrollierte Lockerung der SOP

## 8.1 Zweck

Manchmal soll JavaScript absichtlich Ressourcen einer anderen Origin lesen dürfen, z. B.:
- API-Aufrufe,
- Web Fonts,
- Bilder,
- externe Services.

**CORS – Cross-Origin Resource Sharing** ist eine vom Server kontrollierte Ausnahme zur SOP.

## 8.2 Simple Requests

Bei einfachen Cross-Origin-Requests:
1. Script sendet `GET` oder `POST`.
2. Browser ergänzt:

```http
Origin: https://foo.example
```

3. Zielserver antwortet z. B.:

```http
Access-Control-Allow-Origin: https://foo.example
```

4. Nur wenn die Response die Origin erlaubt, darf JavaScript die Antwort lesen.

## 8.3 Preflighted Requests

Vorab-`OPTIONS`-Anfrage ist nötig, u. a. bei:
- Methoden wie `PUT` oder `DELETE`,
- bestimmten Content-Types wie `text/xml`,
- selbst gesetzten Headern wie `X-PINGOTHER`.

Browser fragt vor dem eigentlichen Request ab, ob Server Methode, Header und Origin erlaubt.

**Klausurfalle:** CORS ist kein Zugriffsschutz für den Server selbst. Es steuert vor allem, ob ein Browser-Script eine Cross-Origin-Response lesen darf. Der Server muss Autorisierung trotzdem selbst prüfen.

---

# 9. OWASP Top 10 2025

OWASP erstellt eine Top-10-Rangliste typischer Risiken in Webanwendungen. Grundlage sind:
- Praxisdaten von Sicherheitsfirmen,
- CWE-Klassifikationen,
- Häufigkeit,
- Ausnutzbarkeit,
- Schadensauswirkung,
- zusätzlich Community-Befragungen.

Ziel: Entwickler sensibilisieren und Prioritäten setzen.

## 9.1 A01: Broken Access Control
> **Definition:** Mangelhafter Zugriffsschutz auf Daten oder Funktionen.

Beispiele:
- leicht erratbare URLs / Forced Browsing: `/admin.php`, `/logs/`
- Parameter-Manipulation: `/kontostand.php?user=alice`
- manipulierte Cookies oder JWTs
- Aktionen ohne notwendige Rechte/Login
- CSRF
- ungeschützte APIs
- fehlerhafte CORS-Konfiguration

Gegenmaßnahmen:
- **Deny by Default / Least Privilege**
- Zugriffskontrolle zentral implementieren
- serverseitige Autorisierungsprüfung bei jeder Anfrage
- Directory Listing deaktivieren
- Backups und nicht benötigte Dateien entfernen
- API-Raten begrenzen
- Fehler/Verstöße protokollieren und alarmieren

## 9.2 A02: Security Misconfiguration
> **Definition:** Unsichere Konfiguration von Web-, Application-, Datenbankservern oder Cloud-Diensten.

Beispiele:
- unnötige Features nicht deaktiviert
- Default-Accounts/Passwörter
- Directory Listing
- fehlende oder falsche Security Header
- Entwicklungs-/Testkonfiguration in Produktion

Gegenmaßnahmen:
- Hardening und minimale Installation
- automatisierte, konsistente Konfiguration
- unterschiedliche Credentials für Dev, Test und Produktion
- Security Reviews und Patchmanagement
- Mandanten-/Komponententrennung
- geeignete HTTP Security Header

## 9.3 A03: Software Supply Chain Failures
> **Definition:** Kompromittierungen im Build-, Verteilungs- oder Update-Prozess, oft über Drittcode, Tools oder Abhängigkeiten.

Beispiele:
- kein Überblick über Dependencies
- veraltete, verwundbare oder nicht mehr unterstützte Komponenten
- Pakete aus kompromittierten Quellen
- keine Security Scans/Patches
- kompromittierte Build- oder Update-Infrastruktur

Gegenmaßnahmen:
- **Software Bill of Materials (SBOM)** erzeugen und pflegen
- transitive Dependencies berücksichtigen
- unnötige Dependencies entfernen
- CVE/NVD/OSV.dev überwachen
- nur offizielle Quellen nutzen
- möglichst signierte Pakete
- CI/CD und IDE aktuell halten
- Dependency Track oder vergleichbare Werkzeuge

## 9.4 A04: Cryptographic Failures
> **Definition:** Daten bei Speicherung oder Übertragung sind nicht angemessen geschützt.

Beispiele:
- Klartextübertragung: HTTP, FTP
- Klartext intern zwischen Load Balancer und Backend
- schwache Algorithmen/Protokolle
- Default Keys
- Schlüssel im Repository
- Passwort direkt als Schlüssel

Gegenmaßnahmen:
- Daten klassifizieren
- aktuelle Algorithmen und Protokolle nutzen
- TLS mit PFS und HSTS
- sichere Speicherung und Übertragung
- Daten löschen, wenn nicht mehr nötig
- Passwort-Hashing mit **Salt + Work Factor**, z. B. PBKDF2

## 9.5 A05: Injection
> **Definition:** Angreifer schleust eigene Befehle oder Code in Interpreter bzw. Ausgabekontexte ein.

Beispiele:
- SQL Injection
- Command Injection
- LDAP Injection
- XSS
- fehlerhafte Eingabevalidierung in Parametern, Headern, URLs, Cookies, JSON, SOAP oder XML

Gegenmaßnahmen:
- sichere APIs und parametrisierte Schnittstellen
- serverseitige Inputvalidierung
- kontextabhängiges Escaping
- statische Codeanalyse
- automatisierte Tests
- Security Tools in CI/CD
- CSP als zusätzliche Schutzschicht

## 9.6 A06: Insecure Design
>**Definition:** Schutzmaßnahme fehlt oder ist unwirksam wegen falscher Entscheidungen in der Designphase, nicht nur wegen fehlerhafter Implementierung.

Beispiele:
- Passwörter werden im Klartext gespeichert
- ein Masterpasswort für Supportzugriffe
- Shop erlaubt Bots den Kauf knapper Produkte ohne Limit

Gegenmaßnahmen:
- Secure Software Development Lifecycle
- Threat Modeling
- mehrschichtige Architektur und Trennung der Schichten
- Mandantentrennung
- Plausibilitätsprüfungen von Frontend bis Backend
- Ressourcenbegrenzung pro Nutzer/Dienst

## 9.7 A07: Authentication Failures
> **Definition:** Fehler bei der Authentifizierung, nicht bei der Autorisierung.

Beispiele:
- Brute Force und Credential Stuffing möglich
- schwache Passwörter
- unsichere Passwort-Resets
- Session-ID in URL/Formularfeld
- Sessions werden nach Logout/Timeout nicht sauber zerstört

Gegenmaßnahmen:
- MFA
- sichere Passwortpolicy
- keine Default-Passwörter
- häufige/bekannte Passwörter gegen Breach-Datenbanken prüfen
- Fehlversuche drosseln, verzögern, loggen, alarmieren
- Session-ID mit hoher Entropie
- Session-ID in `Secure` Cookie
- Session nach Logout/Timeout zuverlässig zerstören

## 9.8 A08: Software or Data Integrity Failures
> **Definition:** Fehlende Integrität von Software, Updateprozessen oder Daten.

Beispiele:
- Libraries/Plugins aus unzuverlässigen Quellen
- unsichere Updates ohne Signaturprüfung
- Packages außerhalb offizieller Quellen
- manipulierbare serialisierte Objekte

Gegenmaßnahmen:
- digitale Signaturen
- SRI bei Dritt-Skripten
- zuverlässige Repositories, z. B. npm/Maven
- definierter Prozess für Code- und Konfigurationsänderungen
- serialisierte Objekte signieren/verschlüsseln oder Manipulation und Replay erkennen

## 9.9 A09: Security Logging and Alerting Failures
> **Definition:** Angriffe werden nicht erkannt oder nicht wirksam behandelt, weil Logging, Monitoring, Alerting oder Prozesse fehlen.

Beispiele:
- Loginversuche oder kritische Transaktionen werden nicht geloggt
- unklare Fehlermeldungen
- Logs sind manipulierbar
- zu viele False Positives
- keine Response-Prozesse

Gegenmaßnahmen:
- Authentifizierungs-, Autorisierungs- und Validierungsfehler mit Kontext loggen
- maschinenlesbares Logformat
- Alarme bei verdächtigem Verhalten
- Monitoring, Playbooks und Incident-Response-Prozesse
- Logs gegen Manipulation schützen

## 9.10 A10: Mishandling of Exceptional Conditions
> **Definition:** Fehlerhafte Behandlung von Ausnahme- und Grenzsituationen verursacht Abstürze, unerwartetes Verhalten oder Sicherheitslücken.

Beispiele:
- fehlende Inputvalidierung
- Fehlerbehandlung zu spät/zu grob
- unerwartete Speicher-, Netzwerk- oder Berechtigungszustände
- inkonsistentes Exception Handling
- Overflows, Race Conditions
- zu ausführliche Fehlermeldungen und Stack Traces

Gegenmaßnahmen:
- „Expect the worst“
- Fehler früh erfassen, sicher behandeln, loggen und ggf. alarmieren
- kritische Schritte atomar ausführen
- Transaktionen bei Fehler zurückrollen
- Rate Limits und Quotas
- strenge Inputvalidierung
- zentrales Exception Handling
- keine sensitiven Fehlerdetails an Nutzer ausgeben

---

# 10. Cross-Site Request Forgery (CSRF)

## 10.1 Prinzip
CSRF nutzt aus, dass der Browser Session-Cookies automatisch an eine Website sendet, bei der das Opfer angemeldet ist.

Ablauf:
1. Opfer ist bei Seite A eingeloggt.
2. Opfer besucht Angreiferseite B.
3. B löst versteckt einen Request an A aus, z. B. über Bild, Form oder JavaScript.
4. Browser sendet Session-Cookie zu A automatisch mit.
5. A behandelt Request als vom Opfer autorisiert.

Beispiel:

```text
https://meinwebmailer.com/delete.php?messageID=123
```

Mögliche Ziele:
- Passwort-/E-Mail-Änderungen
- Löschvorgänge
- Überweisungen
- Router-DNS-Konfiguration: Drive-by-Pharming

## 10.2 Warum POST allein nicht schützt

Ein Angreifer kann auch POST-Requests aus einem versteckten HTML-Formular senden. Entscheidend ist nicht GET oder POST, sondern ein nicht vorhersagbarer zusätzlicher Nachweis.

## 10.3 Gegenmaßnahmen

| Maßnahme                    | Wirkung                                                                                      |
| --------------------------- | -------------------------------------------------------------------------------------------- |
| CSRF Token / Nonce          | Server ordnet Nutzer ein zufälliges Token zu; Token muss mit dem Request mitgesendet werden. |
| Token als Hidden Field      | Server prüft Token im POST-Formular.                                                         |
| Double-Submit Cookie        | Token liegt in Cookie und Formular; Server prüft Gleichheit.                                 |
| Signed Double-Submit Cookie | Token wird zusätzlich per HMAC gegen Manipulation geschützt.                                 |
| SameSite Cookies            | Browser schickt Session-Cookie bei Cross-Site-Szenarien eingeschränkt.                       |
| Origin-/Referer-Prüfung     | Server prüft, ob Request von vertrauenswürdiger Origin kommt.                                |
| Re-Authentisierung/CAPTCHA  | Für besonders kritische Aktionen.                                                            |
| Logout bei Inaktivität      | reduziert missbrauchbare Sessiondauer.                                                       |

**Wichtig:** CSRF-Schutz setzt voraus, dass **XSS ausgeschlossen** ist. XSS kann sonst Tokens aus derselben Origin auslesen.

---

# 11. SameSite Cookies

`SameSite` schränkt ein, wann Browser Cookies bei Cross-Site-Requests senden.

| Wert | Verhalten |
|---|---|
| `None` / nicht gesetzt | Kein SameSite-Schutz. |
| `Lax` | Cookie wird bei bestimmten Cross-Site-Navigationen und sicheren Methoden wie GET, HEAD, OPTIONS, TRACE noch gesendet. |
| `Strict` | Cookie nur senden, wenn Aufruf von derselben Site stammt. Höchster CSRF-Schutz, aber schlechtere Usability. |

Beispiel für `Strict`:
- Nutzer ist bei GitHub eingeloggt.
- Nutzer folgt von einer Blogseite einem Link zu GitHub.
- Cookie wird wegen Cross-Site-Navigation nicht gesendet.
- erneuter Login kann nötig sein.

**Merksatz:** `SameSite=Strict` schützt stärker, kann aber legitime Cross-Site-Nutzung stören.

---

# 12. Cross-Site Scripting (XSS)

## 12.1 Definition

XSS ist Injection von bösartigem JavaScript in eine vertrauenswürdige Website. Der Code wird dann im Browser anderer Nutzer unter der Origin der verwundbaren Website ausgeführt.

Die SOP verhindert das nicht, weil der Code **innerhalb der legitimen Origin** läuft.

## 12.2 Typen

| Typ | Ablauf |
|---|---|
| **Stored / Persistent XSS** | Angreifer speichert Schadcode serverseitig, z. B. Forum/Kommentar. Jeder spätere Besucher führt ihn aus. |
| **Reflected / Non-Persistent XSS** | Opfer klickt präparierten Link; Server reflektiert Parameter ungefiltert in Antwort. |
| **DOM-based XSS** | Clientseitiger Code verarbeitet untrusted Daten, z. B. URL-Parameter, und injiziert sie in DOM. |

## 12.3 Mögliche Folgen

- Cookie-Diebstahl, soweit nicht `HttpOnly`
- Aktionen im Namen des Nutzers
- Phishing innerhalb einer legitimen Seite
- Keylogging
- Exfiltration von Daten
- Manipulation von Seiteninhalt
- Umgehung von CSRF-Schutzmechanismen

## 12.4 Gegenmaßnahmen

- Frameworks nutzen, die standardmäßig sicher escapen, z. B. React/Rails
- **kontextabhängiges Output Encoding/Escaping**
  - HTML-Body
  - HTML-Attribute
  - JavaScript
  - CSS
  - URL
- Bei erlaubtem Markup: Sanitizing, z. B. HTMLPurifier
- alternative Markup-Sprache, z. B. Markdown/Wikitext
- CSP als Defense in Depth
- JavaScript nicht clientseitig pauschal deaktivieren: unpraktisch
- POST statt GET schützt nicht gegen XSS

---

# 13. SQL Injection

## 13.1 Definition

SQL Injection entsteht, wenn untrusted Eingaben durch String-Konkatenation Teil einer SQL-Anweisung werden. Angreifer verändert dadurch die Syntax oder Bedeutung der Query.

Unsicher:

```java
String query =
  "SELECT accounts FROM users WHERE login='" + login +
  "' AND pass='" + password + "'";
```

Sicherer:

```java
String query =
  "SELECT accounts FROM users WHERE login=? AND pass=?";
PreparedStatement statement = conn.prepareStatement(query);
statement.setString(1, login);
statement.setString(2, password);
```

Prepared Statements trennen SQL-Code von Daten.

## 13.2 Angriffsvektoren

- Formfelder
- GET-Parameter
- Cookies
- HTTP-/Netzwerkheader
- Umgebungsvariablen
- **Second-Order Injection**: schädliche Eingabe wird zunächst gespeichert und erst später in anderem Kontext unsicher verwendet.

## 13.3 Arten von SQL Injection

| Typ | Idee / Ziel |
|---|---|
| **Tautology** | Bedingung immer wahr, z. B. `' OR 1=1 --`; Umgehung Authentifizierung oder Datenzugriff. |
| **Union Query** | Ergebnisse anderer Tabellen über `UNION SELECT` auslesen. |
| **Piggy-Backed Query** | Zusätzliche Query einschleusen, z. B. `DROP TABLE`; Manipulation, DoS, ggf. RCE. |
| **Stored Procedure Injection** | Missbrauch DB-seitiger Funktionen; Stored Procedures sind nicht automatisch sicher. |
| **Illegal/Logically Incorrect Queries** | Fehlerausgaben für DB-Fingerprinting und Parameteranalyse nutzen. |
| **Inference / Blind SQLi** | Keine direkte Ausgabe; Verhalten bei true/false oder Timing beobachten. |
| **Timing Attack** | `SLEEP`/`WAITFOR` verursacht messbare Verzögerung. |
| **Alternate Encodings** | Zeichenkodierung/Unicode/Hex zum Umgehen einfacher Filter. |

## 13.4 Gegenmaßnahmen
- Prepared Statements / parameterisierte Queries
- sichere DB-APIs
- Allowlist-Validierung, wo möglich
- minimale DB-Berechtigungen
- keine detaillierten DB-Fehler an Nutzer
- statische Analyse, DAST, automatisierte Tests
- sichere Behandlung aller Eingabekanäle
- Stored Procedures nicht als alleinigen Schutz missverstehen

---

# 14. Content Security Policy (CSP)

## 14.1 Ziel

Browser kann nicht zuverlässig erkennen, ob Code legitimer Anwendungscode oder per XSS injiziert wurde.

Mit **CSP** legt der Server fest:
- welche Ressourcen geladen werden dürfen,
- aus welchen Quellen,
- welche Browserfunktionen eingeschränkt werden.

CSP ersetzt **nicht** Inputvalidierung und Output-Encoding, sondern ergänzt sie.

## 14.2 Setzen einer CSP

Per HTTP-Response-Header:

```http
Content-Security-Policy: default-src 'self'
```

oder per HTML-Meta-Tag:

```html
<meta http-equiv="Content-Security-Policy" content="...">
```

HTTP-Header sind meist die bessere zentrale Variante.

## 14.3 Restriktiver Start

```http
Content-Security-Policy: script-src 'self'
```

Nur Scripts derselben Origin.

```http
Content-Security-Policy: default-src 'self'
```

Alle nicht speziell geregelten Ressourcen nur aus derselben Origin.

Dadurch werden standardmäßig u. a. blockiert:
- Inline-Scripts,
- `eval()` und ähnliche dynamische Codeausführung,
- `javascript:`-Links,
- Inline Event Handler wie `onclick`.

Saubere Alternative:
- JavaScript in externe Dateien,
- Event Handler via `addEventListener`.

## 14.4 Wichtige Direktiven

| Direktive | Zweck |
|---|---|
| `default-src` | Fallback für nicht spezifisch geregelte Ressourcentypen |
| `script-src` | JavaScript-Quellen |
| `style-src` | CSS-Quellen |
| `img-src` | Bilder |
| `font-src` | Web Fonts |
| `connect-src` | Ziele für XHR, Fetch, WebSocket etc. |
| `frame-src` | Quellen für Frames/iFrames |
| `frame-ancestors` | Wer die aktuelle Seite einbetten darf |
| `object-src` | Plugins wie `object`, `embed`, `applet` |
| `media-src` | Audio/Video |
| `form-action` | Zieladressen von Formularen |

## 14.5 Quellangaben

| Ausdruck | Bedeutung |
|---|---|
| `'self'` | Nur gleiche Origin: Protokoll, Host, Port |
| `'none'` | Keine Quelle erlaubt |
| `https://host.example` | Bestimmter Host |
| `data:` | Daten-URI |
| Wildcards | z. B. `https://*.example.org` |
| `'nonce-...'` | Explizit freigegebener Script-Nonce |
| `'sha256-...'` | Hash eines erlaubten Inline-Codefragments |

Gefährliche Ausnahmen:

| Ausdruck | Risiko |
|---|---|
| `'unsafe-inline'` | Inline Scripts/CSS werden erlaubt; schwächt XSS-Schutz. |
| `'unsafe-eval'` | `eval()` und ähnliche dynamische Ausführung werden erlaubt. |

## 14.6 Nonces und Hashes

### Nonce
1. Server erzeugt pro Response einen kryptographisch zufälligen, nicht vorhersagbaren Nonce.
2. Server setzt ihn in CSP.
3. Nur Script-Tags mit passendem Nonce werden ausgeführt.

```http
Content-Security-Policy: script-src 'nonce-gFeZ54'
```

```html
<script nonce="gFeZ54">foo()</script>
```

Nonce muss bei jeder Response neu sein.

### Hash

Server hinterlegt Hash eines konkret erlaubten Inline-Scripts:

```http
Content-Security-Policy: script-src 'sha256-...'
```

Nur exakt passender Code ist erlaubt.

---

# 15. Weitere Web-Schutzmechanismen und Header

## 15.1 Upgrade-Insecure-Requests

Request Header:

```http
Upgrade-Insecure-Requests: 1
```

Client signalisiert, dass sichere Kommunikation bevorzugt wird.

Server kann:
- von HTTP auf HTTPS umleiten,
- HSTS aktivieren.

CSP-Direktive:

```http
Content-Security-Policy: upgrade-insecure-requests;
```

Browser schreibt unsichere Ressourcen-URLs von `http://` nach `https://` um, soweit passend.

Ziel: Schutz gegen Sniffing und Mixed Content.

## 15.2 Origin und Referer

| Header | Inhalt |
|---|---|
| `Referer` | Häufig vollständige vorherige URL, ggf. inklusive Pfad/Parametern. |
| `Origin` | Nur Schema, Host und Port; weniger preisgebend. |

Server kann `Origin`/`Referer` zur CSRF-Erkennung prüfen.

## 15.3 DNT und GPC

### Do Not Track

```http
DNT: 1
```

- `0`: Tracking erlaubt
- `1`: Nutzer möchte nicht getrackt werden
- sonst: keine Präferenz

Praktisch weitgehend bedeutungslos, da Standard nicht verbindlich unterstützt wurde.

### Global Privacy Control

```http
Sec-GPC: 1
```

Ähnliches Konzept, in einigen US-Bundesstaaten rechtlich wirksam.

## 15.4 Schutz gegen Clickjacking

**Clickjacking / UI Redressing:** Angreifer legt eine angegriffene Seite unsichtbar oder überlagert in Frame/iFrame unter harmlosem Inhalt. Opfer klickt scheinbar auf etwas anderes, löst aber Aktion auf echter Seite aus.

Beispiele:
- One-Click-Bestellung
- Likejacking
- Änderung von Sicherheitseinstellungen

### Schutz

```http
X-Frame-Options: DENY
```

| Wert | Wirkung |
|---|---|
| `DENY` | Einbettung in Frames grundsätzlich verboten. |
| `SAMEORIGIN` | Einbettung nur durch gleiche Origin. |
| `ALLOW-FROM ...` | Historische, eingeschränkte Freigabe bestimmter Origins. |

Moderner/zusätzlicher Schutz:

```http
Content-Security-Policy: frame-ancestors 'self'
```

## 15.5 Referrer-Policy

`Referrer-Policy` kontrolliert, welche Informationen als Referer weitergegeben werden.

| Wert | Verhalten |
|---|---|
| `no-referrer` | Nie Referer senden. |
| `no-referrer-when-downgrade` | Nicht von HTTPS zu HTTP senden. |
| `same-origin` | Nur bei gleicher Origin senden. |
| `origin` | Nur Origin senden. |
| `origin-when-cross-origin` | Bei gleicher Origin volle URL, sonst nur Origin. |
| `strict-origin` | Nur Origin, bei HTTPS→HTTP kein Referer. |
| `strict-origin-when-cross-origin` | Bei gleicher Origin volle URL, sonst nur Origin; bei Downgrade nichts. |
| `unsafe-url` | Immer volle URL senden; datenschutzkritisch. |

## 15.6 Permissions Policy

Früher **Feature Policy**.

Kontrolliert Browserfunktionen für Seite und eingebettete Inhalte:
- Kamera
- Mikrofon
- Geolocation
- Sensoren
- Autoplay
- Vibration
- Lautsprecher

Setzung:
- global per `Permissions-Policy` Response Header,
- lokal pro iFrame via `allow`-Attribut.

Nutzen: Least Privilege für Browser-APIs und Drittinhalte.

---

# 16. TLS-Ergänzungen und HSTS

## 16.1 Ausgangspunkt

TLS ermöglicht Vertraulichkeit und Authentizität von HTTP-Verbindungen. Probleme entstehen trotzdem durch:
- unachtsame Nutzer,
- Zertifikatswarnungen werden ignoriert,
- unsaubere Zertifizierungsstellen-Praxis,
- Downgrade-Angriffe.

Genannte Ansätze:
- HTTP Public Key Pinning (HPKP)
- Certificate Transparency
- HTTP Strict Transport Security (HSTS)

## 16.2 HSTS

**HSTS – HTTP Strict Transport Security** schützt gegen HTTPS-Downgrades.

Angriff ohne HSTS:
1. Opfer ruft Login per HTTP auf.
2. MITM verändert Links/Redirects von HTTPS zu HTTP.
3. Angreifer vermittelt HTTPS zum Server, aber HTTP zum Opfer.
4. Opfer merkt Verschlüsselungsverlust eventuell nicht.

HSTS Header:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

| Teil | Bedeutung |
|---|---|
| `max-age` | Zeitraum in Sekunden, in dem Browser nur HTTPS nutzen muss. |
| `includeSubDomains` | Gilt auch für Subdomains. |
| `max-age=0` | HSTS deaktivieren. |

Ablauf:
1. Browser ruft Website zuerst über HTTPS auf.
2. Zertifikat wird geprüft.
3. HSTS Header wird gespeichert.
4. Browser ersetzt künftige `http://example.com`-Aufrufe durch `https://example.com`.
5. Ist Zertifikat ungültig, baut Browser keine Verbindung auf.

Grenzen:
- Erster Aufruf ist ohne HSTS vorher noch angreifbar.
- Nach Ablauf des Zeitraums ebenfalls.
- Lösung: **HSTS Preload List** im Browser.

---

# 17. Zentrale Abgrenzungen

| Begriffspaar | Unterschied |
|---|---|
| Authentifizierung vs. Autorisierung | Authentifizierung = Wer bist du? Autorisierung = Was darfst du? |
| XSS vs. CSRF | XSS führt Code im Kontext der Zielseite aus; CSRF missbraucht automatisch gesendete Sitzungscookies für fremdausgelöste Requests. |
| SOP vs. CORS | SOP blockiert Cross-Origin-Lesezugriff; CORS ist eine vom Zielserver erlaubte Ausnahme. |
| `Secure` vs. `HttpOnly` | `Secure` = Cookie nur über HTTPS; `HttpOnly` = JavaScript darf Cookie nicht auslesen. |
| `SameSite=Lax` vs. `Strict` | Lax erlaubt noch bestimmte Cross-Site-Navigationen; Strict sendet Cookie nur bei Same-Site. |
| SRI vs. CSP | SRI prüft Integrität einer konkreten eingebundenen Ressource; CSP begrenzt Quellen und Ausführungsoptionen grundsätzlich. |
| SQL Injection vs. XSS | SQLi manipuliert DB-Queries serverseitig; XSS injiziert Clientcode in Browser. |
| CORS vs. serverseitige Autorisierung | CORS regelt Browserzugriff auf Responses; Autorisierung muss der Server unabhängig prüfen. |
| HSTS vs. TLS | TLS schützt konkrete Verbindung; HSTS zwingt Browser künftig zur HTTPS-Nutzung. |

---

# 18. Klausur-Checkliste

Du solltest diesen Foliensatz erklären können:

1. Ablauf eines Webaufrufs und typische Multi-Tier-Architektur.
2. Warum HTTP ohne TLS unsicher ist.
3. Session-Verwaltung mit Cookies.
4. Aufbau, Identität und Attribute von Cookies.
5. Domain-, Path-, Secure- und HttpOnly-Regeln.
6. Warum Cookie-Werte nicht vertrauenswürdig sind.
7. Cookie Manipulation, Cookie Theft, Tracking und State Partitioning.
8. Third-Party Cookies, Fingerprinting, Canvas/WebGL/ETag Tracking.
9. Same Origin Policy mit Schema + Host + Port.
10. Risiken eingebundener Dritt-Skripte und `document.domain`.
11. SRI und sein Zweck.
12. DNS Rebinding und DNS Pinning.
13. CORS: Simple Request, Preflight, `Origin`, `Access-Control-Allow-Origin`.
14. Alle OWASP Top 10 2025 Kategorien mit Beispielen/Gegenmaßnahmen.
15. CSRF-Ablauf und Token-/SameSite-/Origin-basierte Gegenmaßnahmen.
16. XSS-Arten und kontextabhängiges Escaping.
17. SQLi-Arten sowie Prepared Statements.
18. CSP-Direktiven, Nonces und Hashes.
19. Clickjacking und `X-Frame-Options` / `frame-ancestors`.
20. Referrer-Policy, Permissions Policy, DNT/GPC.
21. HSTS, Downgrade-Angriff und Preload List.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit 2 – Sicherheit von Web-Anwendungen“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Kapitel: Grundlagen, Sessions/Cookies, Tracking, JavaScript/SOP/CORS, OWASP Top 10 2025, CSP sowie HTTP-Security-Header und HSTS.
