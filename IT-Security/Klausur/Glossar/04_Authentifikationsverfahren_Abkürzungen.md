# IT-Sicherheit 3 – Abkürzungen und Bedeutungen

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit 3 – Authentifikationsverfahren“**.

| Abkürzung | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **2FA** | Two-Factor Authentication | Authentifizierung mit zwei unabhängigen Faktoren, z. B. Passwort + OTP-App. |
| **3FA** | Three-Factor Authentication | Authentifizierung mit drei unabhängigen Faktoren: Wissen, Besitz und Biometrie. |
| **AAI** | Authentication and Authorization Infrastructure | Infrastruktur, die Authentifizierung und Autorisierungsinformationen föderiert bereitstellt, z. B. bei Shibboleth. |
| **AES** | Advanced Encryption Standard | Symmetrisches Verschlüsselungsverfahren; bei RSA SecurID zur Berechnung von Tokencodes genannt. |
| **API** | Application Programming Interface | Programmierschnittstelle, z. B. WebAuthn-API oder Token-Introspection-API. |
| **ASIC** | Application-Specific Integrated Circuit | Spezialhardware für bestimmte Berechnungen; kann Passwort-Cracking stark beschleunigen. |
| **BLE** | Bluetooth Low Energy | Energiesparender Bluetooth-Standard; bei FIDO2 für Kommunikation mit Roaming Authenticators. |
| **CAPTCHA** | Completely Automated Public Turing test to tell Computers and Humans Apart | Mechanismus, um automatisierte Login-/Brute-Force-Versuche zu erschweren. |
| **CTAP / CTAP2** | Client to Authenticator Protocol | Protokoll zwischen Client und FIDO-Authenticator; CTAP2 gehört zu FIDO2. |
| **ECDH** | Elliptic Curve Diffie-Hellman | Asymmetrisches Verfahren für Schlüsselaustausch; im Kontext hybrider Verschlüsselung genannt. |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm | Digitales Signaturverfahren auf elliptischen Kurven; von FIDO2 nutzbar. |
| **FAR** | False Acceptance Rate | Anteil/Häufigkeit, mit der ein unberechtigter Fremder fälschlich akzeptiert wird. |
| **FIDO** | Fast IDentity Online | Familie offener Standards für starke, phishing-resistente Authentifizierung. |
| **FIDO2** | Fast IDentity Online 2 | FIDO-Standardverbund aus WebAuthn und CTAP2. |
| **FPGA** | Field-Programmable Gate Array | Umprogrammierbare Spezialhardware, die u. a. Passwort-Cracking beschleunigen kann. |
| **FRR** | False Rejection Rate | Anteil/Häufigkeit, mit der ein legitimer Nutzer fälschlich abgelehnt wird. |
| **GPU** | Graphics Processing Unit | Grafikkarte bzw. Parallelprozessor; kann Brute-Force-Angriffe stark beschleunigen. |
| **HMAC** | Hash-based Message Authentication Code | Standardisierte MAC-Konstruktion aus Hashfunktion und geheimem Schlüssel; Grundlage von HOTP. |
| **HOTP** | HMAC-Based One-Time Password | Zählerbasiertes Einmalpasswortverfahren. |
| **HSM** | Hardware Security Module | Spezialisierte Hardware zur geschützten Speicherung/Verarbeitung kryptographischer Schlüssel, z. B. Pepper. |
| **IdP** | Identity Provider | Authentifiziert Nutzer und stellt Identitätsinformationen bzw. Assertions für Dienste aus. |
| **JWT** | JSON Web Token | Kompaktes Tokenformat mit Claims; bei OIDC typischerweise für ID Tokens verwendet. |
| **MAC** | Message Authentication Code | Kryptographischer Prüfwert mit gemeinsamem geheimen Schlüssel; bei Challenge-Response/HOTP relevant. |
| **MFA** | Multi-Factor Authentication | Kombination von mindestens zwei unabhängigen Authentifikationsfaktoren. |
| **MITM** | Man-in-the-Middle | Angreifer sitzt zwischen Kommunikationspartnern und kann Kommunikation weiterleiten, mitlesen oder verändern. |
| **NFC** | Near Field Communication | Kurzstreckenfunk; mögliche Verbindung zwischen Client und FIDO2-Authenticator. |
| **NIST** | National Institute of Standards and Technology | US-Standardisierungsbehörde; im Foliensatz für Passwortempfehlungen genannt. |
| **NTLM** | NT LAN Manager | Microsoft-Authentifizierungs-/Hashverfahren; wegen hoher Crack-Geschwindigkeit als schlechtes Beispiel erwähnt. |
| **OIDC** | OpenID Connect | Modernes Authentifizierungsprotokoll auf Basis von OAuth 2.0. |
| **OP** | OpenID Provider | Bezeichnung für den Identity Provider im OpenID-/OIDC-Kontext. |
| **OTP** | One-Time Password | Einmalpasswort, meist nur einmal oder kurzzeitig gültig. |
| **PIN** | Personal Identification Number | Geheimzahl; kann z. B. Hardware-Token oder lokale Passkeys entsperren. |
| **PK** | Public Key | Öffentlicher Schlüssel eines asymmetrischen Schlüsselpaares. |
| **RP** | Relying Party | Dienst/Webserver, der auf eine Authentisierung durch einen IdP oder FIDO-Authenticator vertraut. |
| **RSA** | Rivest-Shamir-Adleman | Asymmetrisches Kryptosystem für Verschlüsselung und Signaturen; bei FIDO2 und RSA SecurID genannt. |
| **SAML** | Security Assertion Markup Language | XML-basiertes Protokoll für föderiertes SSO und Austausch signierter/verschlüsselter Assertions. |
| **SHA-1** | Secure Hash Algorithm 1 | Kryptographische Hashfunktion; in älteren Verfahren und bei k-Anonymity-Checks erwähnt, für neue Sicherheitskonstruktionen nicht empfehlenswert. |
| **SHA-256 / SHA-512** | Secure Hash Algorithm 256/512 | Hashfunktionen; u. a. bei SHA-crypt verwendet. |
| **SSO** | Single Sign-On | Einmalige Anmeldung bei einem Identity Provider für Zugriff auf mehrere Dienste. |
| **TAN** | Transaktionsnummer | Einmalcode zur Bestätigung einer Transaktion, z. B. im Online-Banking. |
| **TOTP** | Time-Based One-Time Password | Zeitbasiertes Einmalpasswortverfahren, meist mit 30-Sekunden-Zeitfenster. |
| **TPM** | Trusted Platform Module | Sicherheitschip im Gerät, der kryptographische Schlüssel geschützt speichern/verarbeiten kann. |
| **U2F** | Universal 2nd Factor | FIDO-Verfahren für einen zusätzlichen starken Faktor, meist mit Hardware-Token. |
| **UAF** | Universal Authentication Factor | FIDO-Verfahren für passwortlose Authentifizierung, oft geschützt durch PIN oder Biometrie. |
| **USB** | Universal Serial Bus | Schnittstelle für externe FIDO-Token. |
| **WAYF** | Where Are You From | Shibboleth-Komponente zur Auswahl der Heimateinrichtung bzw. des passenden Identity Providers. |
| **WebAuthn** | Web Authentication | W3C-Standard/API für Web-basierte Public-Key-Authentifizierung mit FIDO2. |
| **XML** | Extensible Markup Language | Auszeichnungssprache; SAML Assertions sind XML-basiert. |

## Formel- und Protokollkürzel

| Ausdruck | Bedeutung |
|---|---|
| **HOTP(K, C)** | HOTP-Code aus gemeinsamem Schlüssel `K` und Zähler `C`. |
| **TOTP(K, T)** | TOTP-Code aus gemeinsamem Schlüssel `K` und Zeitwert `T`. |
| **T_current** | Aktuelle Zeit in Sekunden. |
| **T_0** | Referenzzeitpunkt, meist Unix Epoch. |
| **X** | Zeitfenster bei TOTP, standardmäßig typischerweise 30 Sekunden. |
| **N** | Rundenzahl / Work Factor bei iterativem Passwort-Hashing. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Identifizierung / Authentifizierung** | Identifizierung nennt die behauptete Identität; Authentifizierung prüft sie. |
| **Authentifizierung / Autorisierung** | Authentifizierung klärt „Wer bist du?“; Autorisierung klärt „Was darfst du?“. |
| **OTP / MFA** | OTP ist ein möglicher Besitzfaktor; erst in Kombination mit einem unabhängigen weiteren Faktor wird daraus MFA. |
| **HOTP / TOTP** | HOTP ist zählerbasiert; TOTP ist zeitbasiert. |
| **Salt / Pepper** | Salt ist individuell und nicht geheim; Pepper ist systemweit und muss getrennt von der Datenbank geheim bleiben. |
| **FAR / FRR** | FAR = Fremder wird akzeptiert; FRR = legitimer Nutzer wird abgelehnt. |
| **U2F / UAF / FIDO2** | U2F = zweiter Faktor; UAF = passwortlos; FIDO2 = WebAuthn + CTAP2 als aktueller Standardverbund. |
| **IdP / RP** | IdP authentifiziert; RP/Dienst vertraut auf die Authentisierung. |
| **SAML / OIDC** | SAML ist XML-basiert und häufig in Enterprise/Hochschulföderationen; OIDC basiert auf OAuth 2.0 und ist typisch für moderne Web-/API-SSO-Szenarien. |
| **ID Token / Access Token / Refresh Token** | ID Token beschreibt Identität; Access Token autorisiert API-Zugriff; Refresh Token erneuert kurzlebige Tokens. |
