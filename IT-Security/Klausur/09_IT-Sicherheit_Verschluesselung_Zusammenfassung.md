# IT-Sicherheit – Verschlüsselung in Netzwerken

> **Foliensatz:** `ITsecFR-2a Verschlüsselung.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** Verschlüsselung auf verschiedenen Ebenen, WLAN-Sicherheit, GSM/UMTS, TLS, IPsec und SSH.

---

# 1. Verschlüsselung im Netzwerk: Einordnung

Verschlüsselung kann im ISO/OSI-Modell auf verschiedenen Ebenen eingesetzt werden.

| Ebene / Ansatz | Beispiele | Typische Eigenschaft |
|---|---|---|
| Leitungsebene | Verschlüsselungsboxen | Schutz einer festen Verbindung zwischen zwei Endpunkten. |
| Luftschnittstelle | WEP, WPA/WPA2/WPA3, GSM, UMTS, LTE/5G | Schutz der drahtlosen Übertragung. |
| Netzwerkschicht | IPsec | Anwendungsunabhängiger Schutz auf IP-Ebene, oft für VPNs. |
| Transportschicht | TLS | Schutz typischer Client-Server-Kommunikation über TCP. |
| Anwendungsschicht | SSH, Mail-/Dateiverschlüsselung | Schutz einzelner Anwendungen bzw. Dienste. |

## 1.1 Leitungsverschlüsselung

Bei der Leitungsverschlüsselung stehen an beiden Enden Verschlüsselungsboxen.

Merkmale:

- einfache Installation,
- starker symmetrischer Algorithmus,
- Geräte sind paarweise abgestimmt,
- physisches Öffnen wird durch gekapselte Hardware erschwert,
- geeignet für sehr hohe Sicherheitsanforderungen, etwa Behörden oder Militär.

---

# 2. WLAN-Sicherheit: WEP, WPA, WPA2 und WPA3

# 2.1 WEP

## Zweck und Aufbau

**WEP – Wired Equivalent Privacy** war ein optionaler Bestandteil von IEEE 802.11.

Es sollte liefern:

- Vertraulichkeit,
- Integrität,
- Authentizität.

Technik:

| Aspekt | Umsetzung |
|---|---|
| Verschlüsselung | RC4 mit 40- oder 104-Bit-Schlüssel |
| Initialisierungsvektor | 24 Bit, wird im Klartext übertragen |
| Integrität | CRC |
| Authentisierung | entweder keine oder Shared-Key-Challenge-Response mit PSK |

Verschlüsselungsprinzip:

```text
C = IV || ((M || CRC(M)) XOR RC4(IV || K))
```

| Symbol | Bedeutung |
|---|---|
| `M` | Klartext |
| `CRC(M)` | Prüfsumme |
| `IV` | Initialisierungsvektor |
| `K` | geheimer WEP-Schlüssel |
| `C` | Chiffrat |

## Schwächen von WEP

| Schwäche | Folge |
|---|---|
| 24-Bit-IV ist zu kurz | IVs wiederholen sich schnell. |
| RC4-/Key-Stream-Schwächen | Aus vielen Paketen kann Schlüsselmaterial abgeleitet werden. |
| Bekannte Klartextmuster | WLAN-Pakete beginnen häufig mit SNAP-Header `0xAAAA03`; erleichtert Angriffe. |
| CRC ist nicht kryptographisch | Manipulation kann unzureichend erkannt werden. |
| Replay möglich | Aufgezeichnete Pakete können erneut gesendet werden. |
| 40-Bit-Schlüssel | Brute Force ist praktisch angreifbar. |
| Shared-Key-Authentisierung | Liefert Angreifern nützliche Klartext-/Chiffrat-Paare. |

## WEP-Angriffe

| Angriff | Kernaussage im Foliensatz |
|---|---|
| Fluhrer-Mantin-Shamir (FMS) | ungefähr 1.000.000 Pakete; damals etwa 40 Minuten. |
| Tews-Pychkine-Weinmann | ungefähr 40.000–100.000 Pakete; damals etwa 1–2 Minuten. |

**Merksatz:** WEP ist obsolet und darf nicht mehr eingesetzt werden.

---

# 2.2 WPA

WPA war eine Übergangslösung für ältere, leistungsschwache Access Points. Es sollte WEP softwareseitig ablösen.

| Baustein | Bedeutung |
|---|---|
| **TKIP** | Temporal Key Integrity Protocol; dynamischere/regelmäßigere Schlüsselverwendung. |
| **MIC** | Message Integrity Check; ergänzt CRC. |
| **Michael** | MIC-Verfahren mit geringem Rechenaufwand. |
| Längerer IV | Erschwert Wiederholungen und dient zusätzlich als Sequenznummer gegen Replay. |
| PSK | Pre-Shared Key für Personal-Modus. |
| EAP | Enterprise Authentication Protocol für Enterprise-Authentisierung. |

WPA nutzt weiterhin RC4 und ist daher keine langfristig sichere Lösung.

Schwächen:

- RC4-Basis,
- begrenzte Schlüsselstärke,
- Dictionary Attacks gegen schwache Passphrasen,
- ARP-Spoofing bleibt als Angriffsweg im LAN möglich.

---

# 2.3 WPA2

WPA2 ersetzt die WPA-/RC4-/TKIP-Basis durch stärkere Verfahren.

| Bereich | Umsetzung laut Foliensatz |
|---|---|
| Verschlüsselung | AES |
| Authentisierung | PSK oder Enterprise mit RADIUS/EAP |
| Einsatz | langfristige Ablösung von WPA |

## KRACK

**KRACK – Key Reinstallation Attack** greift den WPA2-Four-Way-Handshake an.

Ablauf:

1. Angreifer veranlasst, dass Nachricht 3 des Handshakes erneut beim Client ankommt.
2. Client installiert denselben Schlüssel erneut.
3. Paket-Sendezähler wird zurückgesetzt.
4. Empfangs-/Replay-Zähler wird zurückgesetzt.
5. Dadurch entstehen Angriffsoptionen auf den Verkehr.

Gegenmaßnahme:

- Client-/Firmware-Update.

---

# 2.4 WPA3

WPA3 verbessert Authentisierung und Verschlüsselung.

| Verbesserung | Bedeutung |
|---|---|
| Besserer Handshake | Behebt die KRACK-Designproblematik. |
| SAE | Simultaneous Authentication of Equals; ersetzt das klassische PSK-Handshake-Modell. |
| Mehr Schutz bei schwachen Passwörtern | Offline-/Brute-Force-Angriffe werden stärker erschwert. |
| Perfect Forward Secrecy | Kompromittierung eines Langzeitschlüssels soll alte Sitzungen nicht entschlüsseln. |
| Individuelle Verschlüsselung in offenen Netzen | Pro Verbindung eigener Schlüssel, z. B. per Diffie-Hellman. |
| IoT-Onboarding | Vereinfachte Einrichtung, etwa per QR-Code. |
| Höhere Sicherheitsstufen | Unterstützung von 192-Bit-Sicherheitsniveau. |

**Wichtig:** Bei offenen Netzen kann Diffie-Hellman ohne Authentisierung zwar Vertraulichkeit gegen passive Zuhörer liefern, schützt aber nicht automatisch gegen Man-in-the-Middle.

---

# 3. GSM-Sicherheit

## 3.1 Sicherheitsdienste in GSM

| Schutzziel | Umsetzung |
|---|---|
| Zugangskontrolle | PIN schützt die SIM. |
| Authentisierung | Challenge-Response zwischen SIM und Netz. |
| Vertraulichkeit | Sprach- und Signalisierungsdaten werden nach Authentisierung verschlüsselt. |
| Anonymität | TMSI statt dauerhaft sichtbarer Teilnehmerkennung. |

## 3.2 Identitäten und Algorithmen

| Begriff | Bedeutung |
|---|---|
| **SIM** | Subscriber Identity Module |
| **TMSI** | Temporary Mobile Subscriber Identity; temporäre Teilnehmerkennung, bei Location Update neu vergeben und verschlüsselt übertragen. |
| **A3** | Authentisierungsalgorithmus. |
| **A5** | Verschlüsselungsalgorithmus. |
| **A8** | Ableitung des Sitzungsschlüssels. |
| **Ki** | Individueller geheimer Teilnehmer-/Authentisierungsschlüssel. |
| **RAND** | Zufallswert für Challenge-Response. |
| **SRES** | Signed Response. |
| **Kc** | Abgeleiteter Sitzungsschlüssel für die Funkverschlüsselung. |

## 3.3 GSM-Authentisierung

Ablauf:

1. Das Netz besitzt `Ki` im Authentication Center und generiert `RAND`.
2. Netz und SIM berechnen mit A3 aus `Ki` und `RAND` je eine erwartete Antwort:
   ```text
   SRES* = A3(Ki, RAND)
   SRES  = A3(Ki, RAND)
   ```
3. Die SIM sendet `SRES` zurück.
4. Das Netz vergleicht `SRES*` mit `SRES`.
5. Gleichheit bedeutet: Teilnehmer ist authentisiert.

## 3.4 GSM-Verschlüsselung

1. Netz und SIM leiten mit A8 aus `Ki` und `RAND` den Sitzungsschlüssel `Kc` ab:
   ```text
   Kc = A8(Ki, RAND)
   ```
2. Mobilgerät und Basisstation verwenden A5 mit `Kc`, um Datenblöcke zu chiffrieren.

---

# 4. UMTS-Sicherheit

UMTS erweitert GSM um stärkere und stärker getrennte Sicherheitsmechanismen.

## 4.1 Gegenseitige Authentisierung

Die Abbildungen zeigen:

- Kernnetz/AuC berechnet auf Basis von `K` und `RAND`:
  - Authentisierungsdaten `AUTN`,
  - erwartete Antwort `XRES`,
  - Schlüssel `CK` und `IK`.
- Mobilgerät/USIM prüft `AUTN` und berechnet eigene Antwort `RES`.
- Netz vergleicht `RES` mit `XRES`.

**Kernunterschied zu GSM:** UMTS unterstützt auch die Authentisierung des Netzes gegenüber dem Mobilgerät.

## 4.2 Schlüsselableitung

| Funktion | Ergebnis |
|---|---|
| `F3(K, RAND)` | `CK` – Cipher Key |
| `F4(K, RAND)` | `IK` – Integrity Key |

## 4.3 Verschlüsselung

Bei UMTS wird ein Schlüsselstrom mit Funktion `F8` gebildet.

Eingaben:

- `CK`,
- `COUNT-C`,
- `BEARER`,
- `DIRECTION`,
- `LENGTH`.

Der erzeugte Key Stream Block wird per XOR auf die Nutzdaten angewandt.

## 4.4 Integrität

UMTS berechnet mit `F9` einen MAC-I.

Eingaben:

- `IK`,
- Nachricht,
- `COUNT-I`,
- `DIRECTION`,
- `FRESH`.

Empfänger berechnet `XMAC-I` und vergleicht ihn mit `MAC-I`.

**Merksatz:**  
GSM: vor allem Authentisierung und Verschlüsselung.  
UMTS: zusätzlich getrennte Schlüssel und expliziter Integritätsschutz.

---

# 5. TLS / SSL

# 5.1 Einordnung

TLS ist der Nachfolger von SSL und sichert typischerweise TCP-basierte Client-Server-Kommunikation.

Historisch:

- SSLv2/SSLv3 sind obsolet.
- TLS 1.0 und TLS 1.1 gelten ebenfalls als überholt.
- TLS 1.2 und insbesondere TLS 1.3 sind relevante moderne Versionen.

TLS schützt typischerweise:

- Vertraulichkeit,
- Integrität,
- Serverauthentisierung,
- optional Clientauthentisierung.

## 5.2 Session und Verbindung

| Begriff | Bedeutung |
|---|---|
| **TLS Session** | Aushandlung der kryptographischen Algorithmen und eines gemeinsamen Master Secrets. Mehrere Verbindungen können derselben Session zugeordnet sein. |
| **TLS Verbindung** | Eine einzelne TCP-Verbindung; verwendet individuelle, aus dem Master Secret abgeleitete Schlüssel. |

## 5.3 Schutzziele und Bausteine

| Ziel | Mechanismus |
|---|---|
| Authentisierung | X.509-Zertifikate, optional Clientzertifikate. |
| Integrität | MAC/HMAC bzw. moderne AEAD-Modi. |
| Vertraulichkeit | Symmetrische Verschlüsselung. |
| Schlüsselaustausch | RSA oder Diffie-Hellman/ECDHE. |

Mögliche Modi:

- Server authentifiziert, Client anonym: typischer Browser-Webserver-Fall.
- Server und Client authentifiziert: Clientzertifikate.
- Server und Client anonym: kein Schutz gegen MITM.

## 5.4 Cipher Suites lesen

Beispiel:

```text
TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
```

| Bestandteil | Bedeutung |
|---|---|
| `TLS` | Protokollfamilie |
| `ECDHE` | Ephemeral Elliptic Curve Diffie-Hellman; Schlüsselaustausch mit Forward Secrecy |
| `ECDSA` | Signaturalgorithmus auf elliptischen Kurven |
| `AES_256_GCM` | Symmetrische Verschlüsselung im Galois/Counter Mode |
| `SHA384` | Hashfunktion bzw. Teil der Kryptosuite |

## 5.5 TLS-Handshake

Aufgaben:

1. Cipher Suite bzw. kryptographische Verfahren aushandeln.
2. Frische Zufallswerte und geheime Basisinformationen austauschen.
3. Gemeinsames Master Secret bestimmen.
4. Schlüssel für beide Richtungen ableiten.
5. Server und optional Client mittels Zertifikaten authentisieren.
6. Sitzungsinformationen verwalten.

Vereinfacht:

```text
ClientHello
-> unterstützte Versionen, Cipher Suites, Zufallswert

ServerHello + Zertifikat
-> gewählte Suite, Zufallswert, Serveridentität

Schlüsselaustausch
-> gemeinsames Pre-Master-Secret

Master Secret und Verbindungsschlüssel ableiten

Finished-Nachrichten
-> Bestätigung des sicheren Handshakes

Geschützte Nutzdaten
```

## 5.6 Record Layer und TLS-Protokolle

| Teil | Aufgabe |
|---|---|
| Record Protocol | Fragmentiert Daten, schützt sie kryptographisch und überträgt sie. |
| ChangeCipherSpec | Historisch: signalisiert Wechsel auf ausgehandelte Verfahren. |
| Alert Protocol | Fehler-/Warnmeldungen, z. B. fehlgeschlagene MAC-Prüfung. |
| Handshake Protocol | Aushandlung und Authentisierung. |

## 5.7 Diffie-Hellman in TLS

Diffie-Hellman kann statt RSA für den Schlüsselaustausch genutzt werden.

- Server stellt temporären DH-Public-Key bereit.
- Dieser wird typischerweise mit Server-Signaturschlüssel signiert.
- Client und Server berechnen ein gemeinsames Geheimnis.
- Dieses dient als Pre-Master-Secret.

**Wichtig:** Anonymes Diffie-Hellman ohne Zertifikat/Signatur ist gegen Man-in-the-Middle anfällig.

## 5.8 Optionale Clientauthentisierung

1. Server sendet `CertificateRequest`.
2. Client sendet Clientzertifikat.
3. Client signiert die relevanten Handshake-Daten mit Private Key.
4. Server prüft Signatur und Zertifikatskette.

## 5.9 Grenzen und Probleme von TLS

- TLS ersetzt keine digitale Signatur einer einzelnen Geschäftshandlung; daher keine automatische rechtliche Verbindlichkeit.
- Zertifikatswarnungen und Namensabweichungen dürfen nicht ignoriert werden.
- Fehlende Zertifikatsprüfung in Nicht-Browser-Anwendungen ermöglicht MITM.
- Sicherheit hängt von Trust Stores und CAs ab.
- Master Secrets und Session Keys müssen geschützt gespeichert werden.
- TLS-Inspection bzw. Firewalls können mit Ende-zu-Ende-Verschlüsselung kollidieren.

## 5.10 TLS 1.3

Verbesserungen:

- verkürzter Handshake,
- optional 0-RTT für bekannte Verbindungen,
- moderne Kryptographie erzwungen,
- alte Verfahren ausgeschlossen, u. a. RC4, DES, 3DES, MD5, SHA-1 und AES-CBC.

TLS-1.3-Cipher-Suites laut Foliensatz:

```text
TLS_AES_128_GCM_SHA256
TLS_AES_256_GCM_SHA384
TLS_CHACHA20_POLY1305_SHA256
TLS_AES_128_CCM_SHA256
TLS_AES_128_CCM_8_SHA256
```

---

# 6. IPsec

## 6.1 Einordnung und Ziele

**IPsec** ist eine Sicherheitsarchitektur auf Schicht 3.

Ziele:

- Authentizität des Datenursprungs,
- Integrität,
- Vertraulichkeit der IP-Nutzlast,
- Schutz gegen Replay-Angriffe,
- Schlüsselmanagement und Schlüsselwechsel.

Typische Anwendung: VPNs.

## 6.2 Protokolle

| Protokoll | Zweck |
|---|---|
| **AH – Authentication Header** | Integrität und Authentizität von Ursprung/Payload; Sequenznummer gegen Replay; keine Vertraulichkeit. |
| **ESP – Encapsulating Security Payload** | Vertraulichkeit der IP-Daten; optional Integrität/Authentizität mit HMAC und Replay-Schutz. |
| **IKE/IKEv2** | Aushandlung bzw. Verteilung von Schlüsseln und Sicherheitsparametern. |

## 6.3 Transport- und Tunnel-Modus

| Modus | Schutzumfang | Typischer Einsatz |
|---|---|---|
| **Transport Mode** | Absicherung primär der Nutzlast; ursprünglicher IP-Header bleibt sichtbar. | Host-zu-Host. |
| **Tunnel Mode** | Originaler IP-Header und Nutzlast werden vollständig in neues IPsec-Paket eingekapselt. | Gateway-zu-Gateway / VPN. |

Geschachtelte Tunnel sind möglich, z. B. äußerer Tunnel durchs Internet und innerer Tunnel hinter einer Firewall.

## 6.4 SPD, SA und SPI

| Begriff | Bedeutung |
|---|---|
| **SPD – Security Policy Database** | Regelwerk, welche Pakete von wem zu wem mit welchem Verfahren geschützt, verworfen oder durchgelassen werden. |
| **SA – Security Association** | Sicherheitsparameter einer Richtung, etwa Algorithmen, Schlüssel, Lebensdauer und Anti-Replay-Informationen. |
| **SPI – Security Parameters Index** | Kennung im IPsec-Paket; verweist beim Empfänger auf die passende SA. |

Wichtige Eigenschaften:

- SAs sind unidirektional.
- Für beide Richtungen werden separate SAs benötigt.
- Für AH und ESP sind jeweils eigene SAs möglich/nötig.
- SAs liegen in SA-Datenbanken der Kommunikationspartner.

## 6.5 Ablauf beim Versand eines IPsec-Pakets

1. Sender prüft SPD und sucht passende SA.
2. Sender nutzt die SA-Parameter:
   - Verschlüsselung,
   - Hash/MAC,
   - Sequenznummer,
   - Zielparameter.
3. Sender trägt die SPI in den IPsec-Header ein.
4. Empfänger nutzt SPI, um passende SA zu finden.
5. Empfänger prüft Integrität/Replay und entschlüsselt falls nötig.

## 6.6 IKE

**IKEv2 – Internet Key Exchange Version 2** dient dem Schlüsselmanagement.

- handelt Algorithmen und Schlüssel aus bzw. verteilt sie,
- ist nicht zwingend Bestandteil jeder IPsec-Implementierung,
- ist generisch und kann theoretisch auch für andere Protokolle verwendet werden.

## 6.7 IPsec-Fazit

Vorteil:

- Bei korrekter Konfiguration hoher Sicherheitsgrad und anwendungsunabhängiger Schutz auf IP-Ebene.

Nachteile:

- Policy-Konfiguration ist komplex und fehleranfällig.
- Viele Optionen können zu unsicheren Modus-/Algorithmusentscheidungen führen.
- Interoperabilitätsprobleme zwischen Implementierungen.
- IKE über UDP kann durch Firewalls blockiert werden.
- Zusammenspiel mit Firewalls/NAT kann problematisch sein.

---

# 7. Vergleich: TLS und IPsec

| Kriterium | TLS | IPsec |
|---|---|---|
| OSI-Schicht | Transport-/Anwendungsnah | Netzwerkschicht |
| Schutzbereich | Einzelne Client-Server-Verbindung bzw. Anwendung | IP-Verkehr unabhängig von Anwendung |
| Typischer Einsatz | HTTPS, APIs, Webdienste | VPN, Standortkopplung, Remote Access |
| Sichtbarkeit für Anwendung | Anwendung nutzt TLS bewusst | Anwendung bleibt meist unverändert |
| Konfiguration | Zertifikate/Cipher Suites pro Dienst | SPD, SA, IKE, Tunnel/Transport-Modus |
| Hauptvorteil | Sehr verbreitet und webtauglich | Transparenter Schutz für viele Anwendungen |
| Hauptnachteil | Muss pro Anwendung/Verbindung eingesetzt werden | Komplexe Policies und Interoperabilität |

---

# 8. SSH

## 8.1 Einordnung

**SSH2 – Secure Shell** schützt Kommunikation auf Anwendungsebene.

Typische Anwendungen:

- sicheres Remote Login,
- sichere Dateiübertragung,
- sichere Remote-Kommandos,
- Port Forwarding / Tunneling.

## 8.2 Funktionen

| Funktion | Bedeutung |
|---|---|
| Remote Login | Anmeldung per Benutzername/Passwort oder Public Key. |
| Remote Command Execution | Befehle auf entferntem System ausführen. |
| Remote Copying | Dateien sicher kopieren. |
| SFTP | Sichere Dateiübertragung. |
| rsync über SSH | Sichere Synchronisation. |
| sshfs | Entferntes Dateisystem sicher einhängen. |
| Port Forwarding | Lokale oder entfernte Ports durch SSH-Tunnel leiten. |

## 8.3 Vor- und Nachteile

| Vorteil | Nachteil |
|---|---|
| Mehrere Authentisierungsmethoden. | Punkt-zu-Punkt-Lösung, kein allgemeiner Netzwerkschutz. |
| Starke Verschlüsselung, z. B. AES. | Tunnel können Netzwerkgrenzen/Firewall-Regeln umgehen. |
| Sicheres Tunneling vorhandener Dienste. | Schlüssel- und Zugriffsmanagement bleibt nötig. |

## 8.4 Port Forwarding

### Local Forwarding

```bash
ssh -L <lport>:<rhost>:<rport> user@host
```

Ein lokaler Port wird über den SSH-Server zu einem Ziel im entfernten Netz weitergeleitet.

```text
lokaler Client:localhost:lport
-> SSH-Tunnel
-> SSH-Server
-> rhost:rport
```

### Remote Forwarding

```bash
ssh -R <port>:<lhost>:<lport> user@host
```

Ein Port auf dem SSH-Server wird durch den SSH-Tunnel zu einem lokalen Ziel des Clients weitergeleitet.

```text
Nutzer am SSH-Server:port
-> SSH-Tunnel
-> lokaler Client lhost:lport
```

---

# 9. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| WEP / WPA / WPA2 / WPA3 | WEP ist unsicher und obsolet; WPA Übergang mit TKIP/RC4; WPA2 nutzt AES; WPA3 nutzt SAE, verbesserten Handshake und PFS. |
| GSM / UMTS | GSM nutzt A3/A5/A8 und primär Netz-Authentisierung; UMTS ergänzt gegenseitige Authentisierung, getrennte CK/IK und Integritätsschutz. |
| TLS / IPsec | TLS schützt typischerweise bestimmte Client-Server-Verbindungen; IPsec schützt IP-Verkehr unabhängig von Anwendungen. |
| AH / ESP | AH: Integrität/Auth. ohne Vertraulichkeit; ESP: Vertraulichkeit und optional Integrität/Auth. |
| Transport / Tunnel Mode | Transport schützt vor allem Nutzlast; Tunnel kapselt komplettes ursprüngliches IP-Paket. |
| SPD / SA / SPI | SPD = Regelwerk; SA = Parameterzustand einer Richtung; SPI = Verweis auf die passende SA im Paket. |
| TLS Session / TLS Verbindung | Session handelt Parameter/Master Secret aus; Verbindung ist einzelne TCP-Verbindung mit eigenen abgeleiteten Keys. |
| SSH Local / Remote Forwarding | Local: lokaler Port erreicht Remoteziel; Remote: Port auf SSH-Server erreicht lokales Ziel. |

---

# 10. Klausur-Checkliste

Du solltest erklären können:

1. Auf welchen OSI-Schichten Verschlüsselung eingesetzt werden kann.
2. WEP technisch beschreiben und seine Schwächen begründen.
3. WPA, WPA2 und WPA3 vergleichen.
4. KRACK und die Wirkung des Key Reinstallations erklären.
5. SAE und Perfect Forward Secrecy bei WPA3 einordnen.
6. GSM mit SIM, Ki, RAND, SRES, Kc sowie A3/A5/A8 erklären.
7. Warum UMTS gegenüber GSM stärkere Sicherheitsmechanismen besitzt.
8. TLS Session und TLS Verbindung unterscheiden.
9. Aufgaben eines TLS-Handshakes nennen.
10. TLS Cipher Suite wie `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384` lesen.
11. Warum anonymes Diffie-Hellman gegen MITM anfällig ist.
12. TLS 1.3 und ausgeschlossene Legacy-Verfahren nennen.
13. IPsec mit AH, ESP, IKE, SPD, SA und SPI erklären.
14. Transport- und Tunnel-Modus vergleichen.
15. TLS und IPsec vergleichen.
16. SSH-Funktionen sowie Local und Remote Port Forwarding erklären.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Verschlüsselung“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Themen: Leitung/WLAN/Mobilfunk, TLS, IPsec und SSH.
