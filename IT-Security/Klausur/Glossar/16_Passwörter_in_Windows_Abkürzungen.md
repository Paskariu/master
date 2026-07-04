# IT-Sicherheit – Passwörter in Windows: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Passwords in Windows“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **AES** | Advanced Encryption Standard | Symmetrisches Verschlüsselungsverfahren; nicht primär ein Passwort-Hashverfahren. |
| **CPU** | Central Processing Unit | Prozessor; Anzahl/Leistung der CPUs beeinflusst Crackgeschwindigkeit. |
| **GPU** | Graphics Processing Unit | Parallelprozessor/Grafikkarte, die viele schnelle Hashberechnungen parallel durchführen kann. |
| **LM** | LAN Manager | Historisches Windows-Authentisierungs-/Hashformat mit gravierenden Schwächen. |
| **MD5** | Message-Digest Algorithm 5 | Schnelle Hashfunktion; nicht für Passwortspeicherung geeignet. |
| **MFA** | Multi-Factor Authentication | Kombination unabhängiger Authentifizierungsfaktoren. |
| **NTLM** | NT LAN Manager | Windows-Authentisierungs-/Hashverfahren; besser als LM, aber für moderne Passwortspeicherung zu schnell. |
| **PBKDF2** | Password-Based Key Derivation Function 2 | Iteratives Passwort-Hash-/Key-Derivation-Verfahren mit Work Factor. |
| **PWD** | Password | Passwort. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Brute Force** | Systematisches Durchprobieren aller möglichen Passwortkandidaten. |
| **Credential Stuffing** | Versuch, bekannte geleakte Benutzername-Passwort-Kombinationen bei anderen Diensten zu verwenden. |
| **Dictionary Attack / Wörterbuchangriff** | Testen wahrscheinlicher Passwörter aus Listen, Regeln und Leaks. |
| **Hash** | Einweg-Prüfwert fester Länge, der aus einer Eingabe berechnet wird. |
| **Keylogger** | Schadsoftware oder Gerät, das Tastatureingaben aufzeichnet. |
| **LM Hash** | Sehr unsicheres Windows-Legacy-Hashformat mit Großschreibung und 7-Zeichen-Teilung. |
| **Memory-hard** | Passwort-Hashverfahren benötigt viel Arbeitsspeicher und erschwert GPU/ASIC-Massenangriffe. |
| **NTLM Hash** | Windows-Passworthash; für Offline-Cracking schnell berechenbar und daher nicht modern genug. |
| **Offline-Cracking** | Passwortkandidaten werden lokal gegen gestohlene Hashwerte getestet. |
| **Online-Cracking** | Passwortkandidaten werden über reale Loginversuche getestet. |
| **Passphrase** | Längeres Passwort aus mehreren Wörtern, idealerweise zufällig gewählt. |
| **Password Hashing** | Spezielle langsame/memory-hard Verarbeitung von Passwörtern zur sicheren Speicherung. |
| **Pepper** | Zusätzliches geheimes serverseitiges Passwort-Hash-Zusatzgeheimnis, getrennt von der Datenbank. |
| **Rainbow Table** | Vorberechnete Ketten aus Hash- und Reduktionsfunktionen für Angriffe auf ungesalzene schnelle Hashes. |
| **Reduction Function / Reduktionsfunktion** | Deterministische Abbildung eines Hashwerts auf einen Passwortkandidaten; keine Hashumkehrung. |
| **Salt** | Zufälliger, individueller und nicht geheimer Zusatzwert beim Passwort-Hashing. |
| **Time-Memory Trade-off** | Weniger Speicher gegen mehr Rechenaufwand oder umgekehrt. |
| **Work Factor** | Parameter, der Passwort-Hashing absichtlich rechen- bzw. speicheraufwendig macht. |
| **Wörterbuch / Password List** | Sammlung möglicher Passwortkandidaten für Wörterbuchangriffe. |

## Wichtige Verfahren

| Verfahren | Einordnung |
|---|---|
| **Argon2** | Moderne memory-hard Passwort-Hashfunktion; aktuelle Standardempfehlung. |
| **bcrypt** | Adaptives Passwort-Hashverfahren mit konfigurierbarem Work Factor. |
| **scrypt** | Memory-hard Passwort-Hashverfahren. |
| **PBKDF2** | Iteratives Passwort-Hashverfahren; weit verbreitet, aber abhängig von sicherem Parameter-Setup. |
| **MD5 / SHA-1 allein** | Schnelle Hashfunktionen; ungeeignet für Passwortspeicherung. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Hash / Verschlüsselung** | Hash ist nicht zur Rückgewinnung des Passworts gedacht; Verschlüsselung ist mit Schlüssel umkehrbar. |
| **Brute Force / Wörterbuchangriff** | Brute Force testet alle Kombinationen; Wörterbuchangriff testet wahrscheinliche Kandidaten. |
| **Online / Offline-Cracking** | Online wird durch Serverregeln begrenzt; Offline erfolgt lokal gegen gestohlene Hashes. |
| **Vollständige Hash-Tabelle / Rainbow Table** | Vollständige Tabelle speichert viele Paare direkt; Rainbow Table speichert Kettenanfang/-ende. |
| **Salt / Pepper** | Salt ist individuell und darf gespeichert werden; Pepper ist geheim und getrennt von der Datenbank aufzubewahren. |
| **LM Hash / NTLM** | LM ist deutlich schwächer und Legacy; NTLM ist weniger schlecht, aber für heutige Passwortspeicherung nicht ausreichend. |
| **Passwortkomplexität / Passwortentropie** | Formale Komplexitätsregeln können vorhersehbar sein; echte Zufälligkeit und Länge bestimmen die Entropie stärker. |
| **MFA / starkes Passwort** | MFA ergänzt ein Passwort, macht ein schwaches/geleaktes Passwort aber nicht automatisch sicher. |
