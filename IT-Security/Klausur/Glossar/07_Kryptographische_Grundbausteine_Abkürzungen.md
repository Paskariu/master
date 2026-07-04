# IT-Sicherheit – Kryptographische Grundbausteine: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Kryptographische Grundbausteine“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **AES** | Advanced Encryption Standard | Symmetrische Blockchiffre mit 128-, 192- oder 256-Bit-Schlüsseln. |
| **CBC** | Cipher Block Chaining | Betriebsmodus für Blockchiffren; Grundlage von CBC-MAC. |
| **CBC-MAC** | Cipher Block Chaining Message Authentication Code | MAC-Konstruktion auf Basis einer symmetrischen Blockchiffre. |
| **ECC** | Elliptic Curve Cryptography | Kryptographie auf elliptischen Kurven; erreicht Sicherheitsniveaus mit kürzeren Schlüsseln als RSA. |
| **ECC-DSA** | Elliptic Curve Cryptography Digital Signature Algorithm | Digitale Signatur auf Basis elliptischer Kurven, wie im Foliensatz bezeichnet. |
| **HMAC** | Hash-based Message Authentication Code | MAC-Konstruktion aus Hashfunktion und geheimem Schlüssel. |
| **MAC** | Message Authentication Code | Schlüsselabhängiger Prüfwert für Integrität und Authentizität. |
| **PGP** | Pretty Good Privacy | System/Standardfamilie für E-Mail-Verschlüsselung und digitale Signaturen. |
| **RSA** | Rivest-Shamir-Adleman | Asymmetrisches Kryptosystem für Verschlüsselung und digitale Signaturen. |
| **SHA** | Secure Hash Algorithm | Familie kryptographischer Hashfunktionen. |
| **SHA-256** | Secure Hash Algorithm 256 | Hashfunktion mit 256-Bit-Ausgabe. |
| **SHA-512** | Secure Hash Algorithm 512 | Hashfunktion mit 512-Bit-Ausgabe. |
| **SHA-3** | Secure Hash Algorithm 3 | Moderne Hashfamilie auf Basis von Keccak. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Asymmetrische Verschlüsselung** | Verschlüsselung mit Public Key und Entschlüsselung mit zugehörigem Private Key. |
| **Authentizität** | Nachweis, dass Nachricht/Absender tatsächlich von der behaupteten Identität stammt. |
| **Blockchiffre** | Symmetrisches Verschlüsselungsverfahren, das Daten blockweise verarbeitet. |
| **Brute Force** | Systematisches Durchprobieren aller möglichen Schlüssel oder Werte. |
| **Chiffrat** | Verschlüsselte, ohne Schlüssel nicht direkt lesbare Form eines Klartexts. |
| **Digitale Signatur** | Mit Private Key erzeugter, mit Public Key prüfbarer kryptographischer Beweis für Integrität und Authentizität. |
| **Einwegfunktion** | Funktion, deren Ergebnis leicht zu berechnen, deren Umkehrung aber praktisch nicht durchführbar ist. |
| **Hashwert / Message Digest** | Ausgabe einer Hashfunktion mit fester Länge; kryptographischer Fingerabdruck von Daten. |
| **Hybridverschlüsselung** | Kombination: Nachricht symmetrisch, Session Key asymmetrisch verschlüsseln. |
| **Integrität** | Schutz davor, dass Daten unautorisiert und unbemerkt verändert werden. |
| **Kollision** | Zwei unterschiedliche Eingaben erzeugen denselben Hashwert. |
| **Kollisionsresistenz** | Eigenschaft, dass es praktisch unmöglich sein soll, eine Kollision zu finden. |
| **Klartext** | Ursprüngliche lesbare Nachricht vor Verschlüsselung. |
| **Nicht-Abstreitbarkeit** | Möglichkeit, eine Handlung bzw. Urheberschaft später Dritten gegenüber nachzuweisen. |
| **Private Key** | Geheimer Teil eines asymmetrischen Schlüsselpaares. |
| **Public Key** | Öffentlicher Teil eines asymmetrischen Schlüsselpaares. |
| **Session Key** | Kurzlebiger, meist zufällig erzeugter symmetrischer Schlüssel für eine Kommunikationssitzung. |
| **Stromchiffre** | Symmetrisches Verfahren, das Daten kontinuierlich als Bit-/Byte-Strom verarbeitet. |
| **Symmetrische Verschlüsselung** | Ver- und Entschlüsselung mit demselben geheimen Schlüssel. |
| **Vertraulichkeit** | Nur Berechtigte können Informationen lesen. |
| **Verschlüsselung** | Reversible Umwandlung von Klartext in Chiffrat mithilfe eines Schlüssels. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Hash / Verschlüsselung** | Hash ist nicht zur Rückgewinnung des Inputs gedacht; Verschlüsselung ist mit passendem Schlüssel umkehrbar. |
| **Hash / MAC** | Hash ist nicht schlüsselgebunden; MAC hängt von einem gemeinsamen Geheimnis ab. |
| **MAC / digitale Signatur** | MAC kann von beiden Kommunikationspartnern erzeugt werden und ist nicht übertragbar; Signatur nur vom Private-Key-Inhaber und öffentlich prüfbar. |
| **Public Key / Private Key** | Public Key darf verbreitet werden; Private Key bleibt geheim. |
| **Symmetrisch / Hybrid** | Symmetrisch nutzt nur gemeinsames Secret; Hybrid nutzt zusätzlich asymmetrische Kryptographie zur Verteilung des Session Keys. |
