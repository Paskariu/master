# IT-Sicherheit – Kryptographische Grundbausteine

> **Foliensatz:** `wdh-krypto_v2.pdf`  
> **Klausurfokus:** Symmetrische und asymmetrische Verschlüsselung, Hashfunktionen, MACs, hybride Verschlüsselung und digitale Signaturen sauber unterscheiden.

---

# 1. Überblick: Welcher Baustein schützt welches Schutzziel?

| Verfahren | Primäres Schutzziel | Zusätzliche Aussage |
|---|---|---|
| **Symmetrische Verschlüsselung** | Vertraulichkeit | Beide Seiten benötigen denselben geheimen Schlüssel. |
| **Asymmetrische Verschlüsselung** | Vertraulichkeit | In der Praxis meist nur für Schlüsselaustausch bzw. in hybriden Verfahren. |
| **Kryptographische Hashfunktion** | Integrität | Änderungen werden durch Vergleich mit bekanntem Original-Hash erkannt. |
| **MAC** | Integrität, Authentizität | Nur Parteien mit gemeinsamem Geheimnis können ihn erzeugen; kein übertragbarer Urheberschaftsnachweis. |
| **Digitale Signatur** | Integrität, Authentizität | Übertragbarer Nachweis der Urheberschaft; unterstützt Nicht-Abstreitbarkeit. |

**Prüfungsfalle:** Verschlüsselung allein schützt nicht automatisch die Integrität oder Authentizität von Daten.

---

# 2. Symmetrische Verschlüsselung

## 2.1 Prinzip

Alice und Bob besitzen denselben geheimen Schlüssel `K`.

```text
Klartext M --Verschlüsselung mit K--> Chiffrat C
Chiffrat C --Entschlüsselung mit K--> Klartext M
```

- Der Klartext wird durch Verschlüsselung in **Chiffrat** überführt.
- Die Entschlüsselung kehrt den Vorgang um.
- Ohne Kenntnis des geheimen Schlüssels soll ein Angreifer aus dem Chiffrat keine verwertbare Information über den Klartext ableiten können; die Länge kann typischerweise sichtbar bleiben.
- Das Durchprobieren aller Schlüssel (**Brute Force**) muss praktisch unmöglich sein.

## 2.2 Zentrales Problem: Schlüsselverteilung

Beide Kommunikationspartner müssen denselben geheimen Schlüssel besitzen. Dieser Schlüssel muss:

- sicher erzeugt,
- sicher gespeichert,
- sicher ausgetauscht

werden.

Geeignete Aufbewahrung:

- passwortgeschützt,
- in Spezialhardware,
- z. B. in einem Hardware Security Module oder Token.

## 2.3 Konstruktionen

| Konstruktion | Idee |
|---|---|
| **Blockchiffre** | Verarbeitet Daten blockweise. |
| **Stromchiffre** | Verarbeitet Daten fortlaufend als Strom von Bits/Bytes. |

## 2.4 Beispiel: AES

**AES – Advanced Encryption Standard** ist ein verbreitetes symmetrisches Verfahren.

Schlüssellängen:

- 128 Bit
- 192 Bit
- 256 Bit

---

# 3. Asymmetrische Verschlüsselung

## 3.1 Prinzip

Jede Person besitzt ein Schlüsselpaar:

- **Public Key:** darf veröffentlicht werden.
- **Private Key:** muss geheim bleiben.

Für vertrauliche Kommunikation an Bob:

```text
Alice verschlüsselt mit Bobs Public Key.
Bob entschlüsselt mit seinem Private Key.
```

Die zugrunde liegende Einwegbeziehung soll verhindern, dass ein Angreifer den Private Key mit vertretbarem Aufwand aus dem Public Key bestimmt.

## 3.2 Eigenschaften

| Eigenschaft | Bedeutung |
|---|---|
| Trennung von Ver- und Entschlüsselung | Verschlüsselung mit Public Key, Entschlüsselung mit Private Key. |
| Öffentliche Verteilbarkeit | Public Key kann veröffentlicht werden. |
| Schutzbedarf | Private Key muss sicher gespeichert bleiben. |
| Problem | Public Key muss authentisch einer Identität zugeordnet sein; sonst sind Man-in-the-Middle-Angriffe möglich. |

## 3.3 Verfahren und Schlüssellängen

| Verfahren | Typische Größen im Foliensatz |
|---|---|
| **RSA** | 2048 oder 4096 Bit |
| **Elliptic Curve Cryptography (ECC)** | kürzere Schlüssel, z. B. 224 oder 256 Bit |

## 3.4 Praxis: Warum asymmetrisch meist nicht für die komplette Nachricht?

Asymmetrische Verfahren sind im Vergleich zu symmetrischen Verfahren rechenaufwendiger. Daher wird asymmetrische Kryptographie typischerweise für den Austausch bzw. Schutz eines Sitzungsschlüssels verwendet, während die eigentlichen Nutzdaten symmetrisch verschlüsselt werden.

---

# 4. Hybride Verschlüsselung

## 4.1 Prinzip

Hybride Verschlüsselung kombiniert die Vorteile beider Verfahren:

1. Sender erzeugt zufälligen **Session Key**.
2. Die Nachricht wird mit diesem Session Key symmetrisch verschlüsselt.
3. Der Session Key wird mit dem Public Key des Empfängers asymmetrisch verschlüsselt.
4. Sender verschickt:
   - verschlüsselte Nutzdaten,
   - verschlüsselten Session Key.
5. Empfänger entschlüsselt den Session Key mit seinem Private Key.
6. Empfänger entschlüsselt danach die Nachricht symmetrisch.

```text
Nachricht --symmetrisch mit Session Key--> verschlüsselte Daten
Session Key --asymmetrisch mit Public Key Empfänger--> verschlüsselter Session Key
```

## 4.2 Vorteil

| Symmetrisch | Asymmetrisch |
|---|---|
| Schnell für große Datenmengen | Löst das Schlüsselverteilungsproblem |

Hybridverschlüsselung kombiniert daher **Performance** und **sichere Schlüsselverteilung**.

## 4.3 Beispiel: E-Mail mit PGP

Bei E-Mail-Verschlüsselung wird die Nachricht mit einem symmetrischen Session Key verschlüsselt. Dieser Session Key wird anschließend mit dem Public Key des Empfängers geschützt. Nur der Empfänger kann mit seinem Private Key den Session Key wiederherstellen und die Nachricht entschlüsseln.

---

# 5. Kryptographische Hashfunktionen

## 5.1 Prinzip

Eine kryptographische Hashfunktion bildet beliebig lange Eingaben auf einen Hashwert fester Länge ab.

```text
Klartext / Nachricht M --Hashfunktion h--> Hashwert h(M)
```

Der Hashwert heißt auch:

- **Message Digest**
- Fingerabdruck der Daten

Typische Längen:

- 256 Bit
- 512 Bit

Typische Verfahren:

- SHA-256
- SHA-512
- SHA-3

## 5.2 Geforderte Eigenschaften

| Eigenschaft | Bedeutung |
|---|---|
| Einwegfunktion | Zu einem gegebenen Hashwert soll die ursprüngliche Eingabe praktisch nicht berechenbar sein. |
| Änderungsdetektion | Bereits minimale Änderungen der Eingabe sollen zu deutlich anderem Hashwert führen. |
| Kollisionsresistenz | Es soll praktisch unmöglich sein, zwei unterschiedliche Eingaben mit gleichem Hashwert zu finden. |

## 5.3 Einsatz für Integrität

Der Empfänger berechnet selbst den Hash der erhaltenen Nachricht und vergleicht ihn mit einem zuvor bekannten bzw. vertrauenswürdig übermittelten Original-Hash.

```text
h(empfangene Nachricht) == erwarteter Hash ?
```

Gleichheit bedeutet: Die Nachricht wurde mit hoher Wahrscheinlichkeit nicht verändert.

## 5.4 Grenze

Ein Hash allein liefert keine zuverlässige Authentizität:

- Ein Angreifer kann Nachricht **und** Hash gemeinsam ersetzen.
- Daher wird für Authentizität ein MAC oder eine digitale Signatur benötigt.

---

# 6. Message Authentication Code (MAC)

## 6.1 Prinzip

Ein MAC ist ein kryptographischer Prüfwert, der von Nachricht **und** gemeinsamem geheimem Schlüssel abhängt.

```text
MAC = MAC_K(Nachricht)
```

Sender und Empfänger kennen beide denselben geheimen Schlüssel und können beide einen MAC erzeugen und prüfen.

## 6.2 Schutzeigenschaften

Ein korrekter MAC liefert:

- **Integrität:** Nachricht wurde nicht unbemerkt verändert.
- **Authentizität:** Nachricht stammt von einer Partei, die den gemeinsamen Schlüssel kennt.

Ein MAC liefert **keine Nicht-Abstreitbarkeit**:

- Beide Kommunikationspartner können den MAC erzeugen.
- Ein Dritter kann daher nicht beweisen, welche der beiden Parteien ihn erstellt hat.

## 6.3 Typische Konstruktionen

| Konstruktion | Grundlage |
|---|---|
| **HMAC** | Hashfunktion + geheimer Schlüssel |
| **CBC-MAC** | Symmetrische Blockchiffre im CBC-Modus |

---

# 7. Digitale Signaturen

## 7.1 Prinzip

Digitale Signaturen bilden das Prinzip der handschriftlichen Unterschrift kryptographisch nach.

Grundlage ist die Asymmetrie von Public-Key-Kryptographie:

```text
Signatur erzeugen: Private Key
Signatur prüfen:   Public Key
```

Ablauf:

1. Sender berechnet Hash der Nachricht.
2. Sender signiert den Hash mit seinem Private Key.
3. Empfänger berechnet selbst den Hash der erhaltenen Nachricht.
4. Empfänger prüft die Signatur mit dem Public Key des Senders.
5. Beide Hashwerte müssen zusammenpassen.

```text
M --Hash--> h(M) --Signatur mit Private Key--> Signatur
Empfänger: h(M) neu berechnen + Signatur mit Public Key verifizieren
```

## 7.2 Schutzeigenschaften

| Eigenschaft | Warum? |
|---|---|
| **Integrität** | Jede Veränderung der Nachricht erzeugt anderen Hash; Signaturprüfung scheitert. |
| **Authentizität** | Nur Besitzer des Private Key kann die passende Signatur erzeugen. |
| **Nicht-Abstreitbarkeit** | Signatur kann Dritten gegenüber geprüft werden; daher ist sie ein übertragbarer Urheberschaftsnachweis. |

## 7.3 Warum wird der Hash signiert?

Nachrichten können beliebig lang sein. Das direkte Signieren großer Datenmengen wäre ineffizient. Deshalb wird zuerst ein Hash mit fester Länge erzeugt und nur dieser Hash signiert.

## 7.4 Typische Verfahren

- RSA
- Digitale Signaturen auf elliptischen Kurven, z. B. ECC-DSA

---

# 8. MAC und digitale Signatur im Vergleich

| Eigenschaft | MAC | Digitale Signatur |
|---|---|---|
| Schlüsseltyp | Gemeinsamer geheimer Schlüssel | Private/Public-Key-Paar |
| Erzeugen | Alle Parteien mit Shared Secret | Nur Besitzer des Private Key |
| Prüfen | Alle Parteien mit Shared Secret | Jeder mit Public Key |
| Integrität | Ja | Ja |
| Authentizität | Ja, gegenüber Teilnehmern mit Shared Secret | Ja |
| Nicht-Abstreitbarkeit | Nein | Ja, bei authentischer Public-Key-Zuordnung und geschütztem Private Key |
| Übertragbarer Beweis | Nein | Ja |

---

# 9. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Symmetrisch / asymmetrisch | Symmetrisch: gleicher geheimer Schlüssel auf beiden Seiten. Asymmetrisch: Public Key zum Verschlüsseln bzw. Prüfen, Private Key zum Entschlüsseln bzw. Signieren. |
| Verschlüsselung / Hash | Verschlüsselung ist umkehrbar mit Schlüssel; Hashing ist Einwegfunktion. |
| Hash / MAC | Hash nutzt keinen geheimen Schlüssel und schützt allein nicht gegen Austausch durch Angreifer; MAC bindet Hash/Prüfwert an Shared Secret. |
| MAC / digitale Signatur | MAC ist nicht übertragbar, da beide Partner ihn erzeugen können; digitale Signatur ist mit Private Key erzeugt und per Public Key durch Dritte prüfbar. |
| Public Key / Private Key | Public Key ist veröffentlichbar; Private Key muss geheim sein. |
| Langzeitschlüssel / Session Key | Langzeitschlüssel identifiziert/schützt dauerhaft; Session Key wird typischerweise pro Kommunikation erzeugt und für Nutzdaten verwendet. |

---

# 10. Klausur-Checkliste

Du solltest erklären können:

1. Welches Schutzziel symmetrische/asymmetrische Verschlüsselung, Hash, MAC und digitale Signatur liefern.
2. Warum der sichere Austausch eines symmetrischen Schlüssels problematisch ist.
3. Block- und Stromchiffre grob unterscheiden.
4. Warum Public Keys authentisch zugeordnet werden müssen.
5. Wie Hybridverschlüsselung bei E-Mail funktioniert.
6. Was eine kryptographische Hashfunktion ist und welche Eigenschaften sie benötigt.
7. Warum ein Hash allein keine Authentizität garantiert.
8. Wie ein MAC konstruiert werden kann und warum er keine Nicht-Abstreitbarkeit liefert.
9. Wie eine digitale Signatur erzeugt und geprüft wird.
10. Warum Signaturen den Hash und nicht die gesamte Nachricht signieren.
11. MAC und digitale Signatur systematisch vergleichen.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Kryptographische Grundbausteine (Wiederholung)“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Themen: symmetrische/asymmetrische und hybride Verschlüsselung, Hashfunktionen, MACs und digitale Signaturen.
