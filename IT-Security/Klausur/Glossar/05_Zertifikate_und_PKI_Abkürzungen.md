# IT-Sicherheit 4 – Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit 4 – Zertifikate und PKI“**.

## Abkürzungen

| Abkürzung | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ACME** | Automatic Certificate Management Environment | Protokoll zur automatisierten Beantragung, Validierung, Erneuerung und Sperrung von Zertifikaten. |
| **AIA** | Authority Information Access | X.509-Erweiterung mit Fundort des CA-Zertifikats und/oder OCSP-Responders. |
| **AKI** | Authority Key Identifier | X.509-Erweiterung zur Kennzeichnung des Schlüssels der ausstellenden CA. |
| **CA** | Certification Authority | Zertifizierungsstelle, die Zertifikate ausstellt und signiert. |
| **CBC** | Cipher Block Chaining | Blockchiffre-Modus; ohne Integritätsschutz formbar und im EFAIL-Kontext problematisch. |
| **CN** | Common Name | Bestandteil eines X.500 Distinguished Name. |
| **CRL** | Certificate Revocation List | Signierte Liste gesperrter Zertifikate. |
| **CSR** | Certificate Signing Request | Signierte Zertifikatsanfrage, z. B. nach PKCS#10. |
| **CT** | Certificate Transparency | Öffentliches, kryptographisch gesichertes Logsystem für ausgestellte TLS-Zertifikate. |
| **DN** | Distinguished Name | Strukturierte Bezeichnung einer Entität, z. B. `CN`, `O`, `OU`, `C`. |
| **EE** | End Entity | Endnutzer, Person oder System mit Endzertifikat; stellt üblicherweise keine Zertifikate aus. |
| **EKU** | Extended Key Usage | X.509-Erweiterung für konkrete Einsatzzwecke wie TLS Server Auth oder Code Signing. |
| **GnuPG / GPG** | GNU Privacy Guard | Open-Source-Implementierung von OpenPGP. |
| **HPKP** | HTTP Public Key Pinning | Historischer Mechanismus zum Pinnen erwarteter TLS-Schlüssel; heute nicht mehr empfohlen/unterstützt. |
| **HSM** | Hardware Security Module | Spezialisierte Hardware für geschützte Schlüsselverwaltung. |
| **HSTS** | HTTP Strict Transport Security | HTTP-Mechanismus, der Browser für eine Domain zur HTTPS-Nutzung verpflichtet. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll; ohne TLS nicht gegen Abhören/Manipulation geschützt. |
| **HTTPS** | HTTP over TLS | HTTP über TLS-gesicherte Verbindung. |
| **IPRA** | Internet Policy Registration Authority | Oberste Policy-Ebene im dargestellten streng hierarchischen PKI-Modell. |
| **ITU** | International Telecommunication Union | Internationale Organisation; Herausgeber von X.509. |
| **JWS** | JSON Web Signature | Format für digital signierte JSON-Nachrichten; bei ACME verwendet. |
| **LDAP** | Lightweight Directory Access Protocol | Verzeichnisprotokoll, z. B. für Zertifikate und CRLs. |
| **OCSP** | Online Certificate Status Protocol | Online-Abfrage des Sperrstatus eines einzelnen Zertifikats. |
| **OID** | Object Identifier | Globale Kennung für Algorithmen, Erweiterungen und PKI-Objekte. |
| **OpenPGP** | Open Pretty Good Privacy | Offener Standard für PGP-Nachrichten und -Zertifikate. |
| **OU** | Organizational Unit | Organisationseinheit als Bestandteil eines Distinguished Name. |
| **PCA** | Policy Certification Authority | CA-Ebene für unterschiedliche Zertifikats-Policies in einer hierarchischen PKI. |
| **PEM** | Privacy-Enhanced Mail | Historisches PKI-Konzept bzw. häufiges Textformat für Zertifikate/Keys. |
| **PGP** | Pretty Good Privacy | System/Standardfamilie für E-Mail-Verschlüsselung, Signaturen und Web of Trust. |
| **PKCS#10** | Public-Key Cryptography Standards #10 | Standardformat für Certificate Signing Requests. |
| **PKCS#12** | Public-Key Cryptography Standards #12 | Containerformat für Private Keys, Zertifikate und ggf. Zertifikatsketten. |
| **PKI** | Public Key Infrastructure | Infrastruktur für Ausstellung, Verteilung, Prüfung und Widerruf von Zertifikaten. |
| **PSE** | Personal Security Environment | Sicherer Speicher für Private Keys, z. B. Token, Smartcard oder HSM. |
| **RA** | Registration Authority | Registrierungsstelle, die Identitäten von Zertifikatsantragstellern prüft. |
| **RFC** | Request for Comments | Reihe technischer Spezifikationen und Internetstandards. |
| **RSA** | Rivest-Shamir-Adleman | Asymmetrisches Kryptosystem für Verschlüsselung und digitale Signaturen. |
| **SAN** | Subject Alternative Name | X.509-Erweiterung für alternative Namen wie DNS-Namen, E-Mail oder IP. |
| **SCT** | Signed Certificate Timestamp | Kryptographische Bestätigung, dass ein Zertifikat in ein CT-Log aufgenommen wurde. |
| **SKI** | Subject Key Identifier | X.509-Erweiterung zur Kennzeichnung des Public Keys des Zertifikatsinhabers. |
| **TLS** | Transport Layer Security | Kryptographisches Protokoll für sichere Netzwerkverbindungen. |
| **TOFU** | Trust on First Use | Vertrauen in beim ersten Kontakt gesehenen Key; spätere Key-Änderung wird erkannt. |
| **URI** | Uniform Resource Identifier | Kennung einer Ressource, z. B. URL als Zertifikatsattribut. |
| **URL** | Uniform Resource Locator | Webadresse. |
| **X.500** | X.500 Directory Services | Standardfamilie für Verzeichnisdienste; Ursprung der DN-Syntax. |
| **X.509** | X.509 Public Key Certificates | Standard für Public-Key-Zertifikate und Zertifikatspfadvalidierung. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Basic Constraints** | X.509-Erweiterung, die festlegt, ob Zertifikatsinhaber eine CA ist. |
| **Bridge CA** | CA, die mehrere sonst unabhängige PKI-Hierarchien über Zertifikate verbindet. |
| **Certificate Policy** | Aussage darüber, unter welchen Bedingungen ein Zertifikat ausgestellt wurde und wofür es gilt. |
| **Certificate Transparency Log** | Öffentliches append-only Log für Zertifikate, kryptographisch abgesichert z. B. über Merkle Trees. |
| **Certificate Chain / Zertifikatskette** | Folge von Zertifikaten vom Endzertifikat über Aussteller bis zu einem Trust Anchor. |
| **Cross-Zertifizierung** | Gegenseitige Zertifizierung/Vertrauensverbindung zwischen unabhängigen CAs. |
| **Direct Trust** | Authentizitätsprüfung eines Public Keys durch direkten, vertrauenswürdigen Kontakt. |
| **EFAIL** | Angriffsklasse gegen PGP/S/MIME-Mailclients, die HTML-Rendering, externe Ressourcen und ggf. Chiffrat-Manipulation zur Klartext-Exfiltration nutzt. |
| **Endzertifikat** | Zertifikat einer End Entity, etwa eines Webservers oder einer Person. |
| **Fingerprint** | Hashwert eines Public Keys bzw. Zertifikats zur kompakten Authentizitätsprüfung über separaten Kanal. |
| **Key Continuity** | Akzeptiert beim ersten Kontakt gesehenen Key und warnt bei späteren Änderungen; z. B. SSH. |
| **Key Usage** | X.509-Erweiterung, die zulässige kryptographische Schlüsselverwendungen definiert. |
| **Merkle Tree** | Hashbaum zur manipulationssicheren Protokollierung großer Datenmengen, z. B. CT-Logs. |
| **Mesh PKI** | Mehrere unabhängige PKI-Hierarchien mit Cross- oder Bridge-Zertifizierung. |
| **Owner Trust** | Lokale Einschätzung, wie sorgfältig ein Schlüsselinhaber andere Schlüssel beglaubigt. |
| **Private Key** | Geheimer Schlüssel eines asymmetrischen Paars; muss geschützt gespeichert werden. |
| **Public Key** | Öffentlicher Schlüssel eines asymmetrischen Paars; benötigt authentische Zuordnung zur Identität. |
| **Root CA** | Oberste CA einer Hierarchie, deren Public Key als Trust Anchor vorliegt. |
| **Sub-CA** | Untergeordnete CA, die von einer Root- oder anderen Sub-CA zertifiziert wurde. |
| **Trust Anchor** | Authentischer, als vertrauenswürdig angenommener Public Key einer obersten CA. |
| **Trust List / Root Store** | Liste vertrauenswürdiger Root-CA-Zertifikate in Browsern oder Betriebssystemen. |
| **Trust Signature** | OpenPGP-Signatur, die zusätzlich Aussage über Vertrauenswürdigkeit als Introducer enthält. |
| **Trusted Introducer** | Im Web of Trust vertrauter Schlüsselinhaber, dessen Beglaubigungen für andere Schlüssel berücksichtigt werden. |
| **Web of Trust** | Dezentrales Vertrauensmodell in OpenPGP, bei dem Nutzer Schlüssel gegenseitig signieren. |
| **Whitelisting** | Hier: Zertifikate, die nicht mehr gültig sein sollen, werden aus einem Verzeichnis entfernt; selten als alleinige Sperrmethode. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **CA / RA** | CA stellt Zertifikate aus; RA übernimmt Identitätsprüfung. |
| **CRL / OCSP** | CRL liefert Liste gesperrter Zertifikate; OCSP liefert Status eines einzelnen Zertifikats online. |
| **Subject / Issuer** | Subject ist Inhaber des Public Keys; Issuer ist Aussteller/Signierer des Zertifikats. |
| **Trust Anchor / Root CA** | Root CA ist meist Trust Anchor; Trust Anchor bezeichnet präziser den als authentisch vorliegenden Root-Public-Key. |
| **Owner Trust / Key Validity** | Owner Trust bewertet Sorgfalt des Ausstellers; Key Validity bewertet Authentizität eines Zielschlüssels. |
| **Key Usage / Extended Key Usage** | Key Usage = allgemeine kryptographische Operationen; EKU = konkreter Anwendungskontext. |
| **HPKP / CT** | HPKP pinnt erwartete Keys im Browser; CT protokolliert Zertifikatsausstellung öffentlich. |
| **PGP / OpenPGP / GnuPG** | PGP ist ursprüngliches System; OpenPGP ist offener Standard; GnuPG ist Implementierung. |
| **HSTS / TLS** | TLS schützt eine Verbindung; HSTS sorgt dafür, dass Browser künftig nur TLS/HTTPS verwenden. |
