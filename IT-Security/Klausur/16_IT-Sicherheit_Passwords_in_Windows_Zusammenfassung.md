# IT-Sicherheit – Passwörter in Windows und Passwort-Cracking

> **Foliensatz:** `ITsecFR-Windows Passwords.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** Passwort-Hashes, Offline-Cracking, Brute Force, Wörterbuchangriffe, Rainbow Tables, LM/NTLM sowie Wirkung von Salt, Passwortlänge und Passwort-Hashverfahren.

---

# 1. Grundidee: Passwörter und Passwort-Hashes

## 1.1 Passwortauthentisierung

Passwörter sind ein Standardverfahren zur Authentifizierung.

Bei einer Anmeldung wird das Passwort nicht idealerweise direkt gespeichert oder verglichen. Stattdessen:

1. Nutzer gibt Passwort ein.
2. System berechnet daraus mit einem Passwort-Hashverfahren einen Hashwert.
3. System vergleicht diesen Hash mit dem gespeicherten Hashwert.
4. Stimmen beide überein, wird die Anmeldung akzeptiert.

```text
Passwort -> Passwort-Hashfunktion -> gespeicherter/verglichener Hashwert
```

**Wichtig:** Systeme sollten Passwörter nicht im Klartext speichern.

## 1.2 Warum ein gestohlener Hash gefährlich ist

Hat ein Angreifer Passwort-Hashes, kann er Passwortkandidaten lokal testen:

```text
Kandidat -> Hash berechnen -> mit gestohlenem Hash vergleichen
```

Das ist **Offline-Cracking**. Es unterliegt nicht denselben Rate Limits oder Account-Sperren wie ein Online-Login.

**Merksatz:** Die Sicherheit eines Passworts hängt nicht nur von dessen Stärke, sondern entscheidend auch von der Speicherart und dem verwendeten Hashverfahren ab.

---

# 2. Wege zur Passwortgewinnung

Der Foliensatz nennt mehrere Angriffswege.

| Angriff | Prinzip |
|---|---|
| Raten / Beobachten | Passwort wird erraten oder bei der Eingabe beobachtet. |
| Keylogger | Schadsoftware zeichnet Tastatureingaben auf. |
| Social Engineering | Nutzer wird manipuliert, Passwort oder andere Zugangsdaten preiszugeben. |
| Brute Force | Alle möglichen Zeichenkombinationen werden systematisch ausprobiert. |
| Wörterbuchangriff | Kandidaten aus Passwortlisten, Leaks, Namen und typischen Mustern werden getestet. |
| Rainbow Tables | Vorberechnete Hash-Ketten werden zur schnellen Zuordnung eines Hashs zu möglichen Passwörtern verwendet. |

## 2.1 Online- und Offline-Angriffe

| Angriffstyp | Eigenschaften | Gegenmaßnahmen |
|---|---|---|
| **Online-Cracking** | Loginversuche direkt gegen ein System; durch Netzwerk, Rate Limits und Sperren gebremst. | MFA, Rate Limiting, Verzögerungen, Lockouts, Monitoring. |
| **Offline-Cracking** | Angreifer besitzt Hashdatenbank; Tests lokal und hochparallel möglich. | Salt, Pepper, langsame/memory-hard Passwort-Hashverfahren, lange einzigartige Passphrasen. |

---

# 3. Brute Force und Passwortkomplexität

## 3.1 Anzahl möglicher Passwörter

Bei einem Zeichenvorrat von etwa 62 Zeichen:

```text
26 Kleinbuchstaben
+ 26 Großbuchstaben
+ 10 Ziffern
= 62 Zeichen
```

ergibt sich für ein Passwort der Länge `n`:

```text
Anzahl möglicher Passwörter = Zeichenvorrat^n
```

Beispiel:

```text
62^6 = 56.800.235.584
62^8 = 218.340.105.584.896
```

Sonderzeichen vergrößern den Zeichenvorrat weiter.

## 3.2 Einfluss der Länge

Jedes zusätzliche Zeichen multipliziert den Suchraum mit der Anzahl möglicher Zeichen.

Bei 62 möglichen Zeichen:

```text
ein Zeichen mehr = 62-mal mehr Kombinationen
```

Deshalb wirkt Passwortlänge stark gegen Brute-Force-Angriffe.

## 3.3 Einfluss des Hashverfahrens und der Hardware

Die Folien zeigen historische Rechnungen und aktuelle Diagramme zu Crackzeiten. Die exakten Zeiten hängen stark ab von:

- Zeichenmenge,
- Passwortlänge,
- ob das Passwort wirklich zufällig ist,
- Hashverfahren,
- Work Factor,
- Salt,
- eingesetzter Hardware,
- Angriffsmethode.

Die Diagramme auf Seiten 16–17 verdeutlichen besonders:

- kurze Passwörter sind selbst bei bcrypt mit Work Factor 10 oft deutlich schneller angreifbar als erwartet,
- mehr Länge und ein größerer Zeichensatz erhöhen die benötigte Zeit stark,
- leistungsfähige GPU-/Spezialhardware verändert die Angriffszeit erheblich.

**Klausurpunkt:** Crackzeit ist keine feste Eigenschaft einer Länge allein. Sie hängt immer von Passwortentropie, Hashverfahren und Angreiferressourcen ab.

---

# 4. Wörterbuchangriffe

## 4.1 Prinzip

Beim Wörterbuchangriff testet ein Angreifer keine vollständige Zeichenmenge, sondern wahrscheinliche Kandidaten:

- häufige Passwörter,
- Namen,
- Wörter,
- Tastaturmuster,
- bekannte Leaks,
- Variationen wie `Passwort1!`.

Die Folien nennen große Passwortlisten wie Ramius und CrackStation als Beispiele.

## 4.2 Warum Wörterbuchangriffe effektiv sind

Viele Nutzer wählen keine zufälligen Passwörter. Daher ist ein Wörterbuchangriff oft viel effizienter als vollständiges Brute Force.

Beispiel:

```text
Sommer2026!
```

hat zwar Groß-/Kleinbuchstaben, Ziffern und Symbol, ist aber ein typisches Muster und kann regelbasiert früh getestet werden.

## 4.3 Gegenmaßnahmen

- lange, zufällig erzeugte Passphrasen,
- Passwortmanager,
- keine Wiederverwendung,
- Prüfung gegen bekannte Passwortleaks / Deny Lists,
- MFA,
- sichere Passwort-Hashspeicherung.

---

# 5. Vorberechnete Hash-Tabellen

## 5.1 Vollständige Hash-Tabellen

Ein Angriff kann alle möglichen Passwortkandidaten vorab hashen und speichern:

```text
Passwort -> Hash
```

Die Tabelle wird nach Hashwert sortiert. Bei einem später gestohlenen Hash kann der passende Passwortwert sehr schnell nachgeschlagen werden.

Vorteil:

- hoher Vorbereitungsaufwand,
- danach sehr schnelle Lookup-Phase.

Nachteil:

- hoher Speicherbedarf.

Die Folien zeigen dazu historische Größenordnungen: Bereits für kurze Passwörter können Tabellen sehr groß werden.

## 5.2 Time-Memory Trade-off

Ein **Time-Memory Trade-off** tauscht Speicherbedarf gegen zusätzliche Rechenzeit.

Statt jeden Hashwert direkt zu speichern, werden Berechnungsketten oder komprimierte Strukturen genutzt.

Ziel:

```text
weniger Speicher
gegen
mehr Rechenaufwand beim Wiederfinden
```

Rainbow Tables sind ein wichtiger Vertreter dieses Prinzips.

---

# 6. Rainbow Tables

## 6.1 Grundprinzip

Rainbow Tables sind vorberechnete Ketten aus Hash- und Reduktionsfunktionen.

Schematisch:

```text
Passwort
-> Hashfunktion H
-> Reduktionsfunktion R1
-> neuer Passwortkandidat
-> Hashfunktion H
-> Reduktionsfunktion R2
-> ...
```

Gespeichert werden nicht alle Zwischenwerte, sondern typischerweise nur:

```text
Startwert der Kette
+ Endwert der Kette
```

Die Folie auf Seite 7 zeigt dieses Prinzip mit Ketten für Passwörter `0..9`.

## 6.2 Reduktionsfunktion

Eine Reduktionsfunktion ist keine Entschlüsselung des Hashs. Sie bildet einen Hashwert deterministisch auf einen neuen Passwortkandidaten ab.

```text
Hashwert -> Kandidat aus Passwortsuchraum
```

Dadurch kann eine Kette weitergeführt werden.

## 6.3 Angriff mit Rainbow Table

Vereinfacht:

1. Angreifer erhält Zielhash.
2. Er probiert, ob der Hash an verschiedenen Positionen einer Kette liegen könnte.
3. Er wendet passende Reduktions-/Hashschritte an und prüft, ob ein gespeicherter Kettenendwert entsteht.
4. Bei Treffer rekonstruiert er die passende Kette ab dem Startwert.
5. Er sucht darin den Zielhash und erhält den zugehörigen Passwortkandidaten.

## 6.4 Vorteil und Grenze

| Vorteil | Grenze |
|---|---|
| Deutlich weniger Speicher als vollständige Hash-Tabellen. | Mehr Rechenaufwand bei der Suche. |
| Sehr effektiv gegen schnelle, ungesalzene Hashverfahren. | Gegen individuelle Salts praktisch stark entwertet. |
| Vorberechnung kann für viele Hashes wiederverwendet werden. | Deckt nur den vorberechneten Zeichensatz und Längenbereich ab. |

**Merksatz:** Rainbow Tables sind besonders gefährlich bei schnellen, ungesalzenen Passwort-Hashes.

---

# 7. Wirkung von Salt

## 7.1 Definition

Ein **Salt** ist ein zufälliger Zusatzwert, der pro Passwort bzw. pro Nutzer verwendet wird.

```text
Hash = PasswordHash(Salt || Passwort)
```

Der Salt wird mit dem Hash gespeichert und muss nicht geheim sein.

## 7.2 Wirkung

Ein Salt bewirkt:

- gleiche Passwörter erzeugen unterschiedliche Hashwerte,
- vorberechnete Tabellen sind nicht direkt wiederverwendbar,
- Rainbow Tables müssten für jeden Salt separat erstellt werden,
- Massenangriffe auf viele Accounts werden deutlich teurer.

Die Folien nennen Linux als Beispiel, bei dem Salt Rainbow Tables erheblich erschwert.

## 7.3 Wichtige Einordnung

Ein Salt:

- macht ein schwaches Passwort nicht stark,
- verhindert kein gezieltes Offline-Cracking,
- verhindert aber effiziente Vorberechnung und parallele Wiederverwendung von Angriffstabellen.

Für moderne Passwortspeicherung werden zusätzlich langsame bzw. memory-hard Verfahren wie **Argon2**, **scrypt**, **bcrypt** oder **PBKDF2** eingesetzt.

---

# 8. Windows-spezifische Passwort-Hashes

## 8.1 LM Hash

**LM Hash** ist ein historisches Windows-Hashverfahren und unsicher.

Zentrale Schwächen:

- Passwort wird in Großbuchstaben umgewandelt,
- Passwort wird in zwei Teile zu je maximal sieben Zeichen aufgeteilt,
- beide Teile werden getrennt verarbeitet,
- dadurch wird ein langes Passwort effektiv in zwei kleinere Suchprobleme zerlegt,
- moderne Rainbow Tables und Brute Force können LM Hashes sehr effizient angreifen.

Die Folien nennen für LM Hash umfangreiche vorgefertigte Rainbow Tables.

**Merksatz:** LM Hashes dürfen nicht mehr verwendet werden.

## 8.2 NTLM

**NTLM** ist ein Windows-Authentisierungs-/Hashverfahren und deutlich besser als LM, aber für Passwortspeicherung nach modernen Maßstäben zu schnell.

Risiken:

- schnelle Offline-Prüfung von Passwortkandidaten,
- anfällig für Wörterbuch- und Brute-Force-Angriffe bei gestohlenen Hashes,
- kein moderner memory-hard Schutz.

**Wichtig:** NTLM-Hashing ist nicht dasselbe wie ein moderner Passwort-Hash wie Argon2 oder bcrypt.

## 8.3 Fehlende LM-Hashes

Der Foliensatz nennt eine Windows-Registry-Einstellung, um Speicherung von LM Hashes zu verhindern:

```text
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa
"nolmhash" = dword:00000001
```

Die konkrete Aktivierung/Verfügbarkeit hängt von Windows-Version und Unternehmensrichtlinien ab. Klausurrelevant ist vor allem die Aussage:

> LM Hashes deaktivieren bzw. nicht speichern.

---

# 9. Beispielwerkzeuge aus dem Foliensatz

| Werkzeug / Quelle | Einordnung |
|---|---|
| **Ophcrack** | Historisches Tool zum Cracken insbesondere von LM-/Windows-Hashes mithilfe von Rainbow Tables. |
| **RainbowCrack / Rainbow Tables** | Werkzeuge bzw. Tabellen für vorberechnete Hash-Ketten. |
| Online Hash Cracking Services | Externe Dienste, die Hashwerte gegen eigene Datenbanken/Angriffsressourcen prüfen können. |
| Passwortlisten | Große Datenbanken mit Kandidaten für Wörterbuchangriffe. |

**Sicherheitsrelevanz:** Hashes und Passwortdaten gehören nicht in externe Dienste, außer dies erfolgt in ausdrücklich autorisierten Testumgebungen ohne reale Credentials.

---

# 10. Praktische Schutzmaßnahmen

## 10.1 Für Nutzer

- lange, einzigartige Passphrasen oder zufällige Passwörter verwenden,
- Passwortmanager einsetzen,
- keine Passwörter wiederverwenden,
- MFA aktivieren,
- Phishing und Social Engineering beachten,
- bei Eingabe auf Keylogger-/Geräterisiken achten,
- Wiederherstellungs- und Backup-Strategien haben.

## 10.2 Für Systembetreiber

| Maßnahme | Wirkung |
|---|---|
| Keine LM Hashes | Verhindert Nutzung eines besonders schwachen Legacy-Hashformats. |
| Moderne Passwort-Hashverfahren | Argon2, scrypt, bcrypt oder PBKDF2 mit geeignetem Work Factor verlangsamen Offline-Cracking. |
| Individueller Salt | Verhindert effiziente Wiederverwendung von Rainbow Tables. |
| Optional Pepper | Zusätzliches serverseitiges Geheimnis, getrennt von Datenbank speichern. |
| MFA | Begrenzung der Wirkung gestohlener Passwörter. |
| Rate Limiting und Monitoring | Schutz vor Online-Brute-Force und Credential Stuffing. |
| Passwort-Deny-Lists | Bekannte kompromittierte bzw. triviale Passwörter ablehnen. |
| Keine unnötigen Passwortwechsel | Wechsel bei Verdacht/Leak, nicht pauschal ohne Anlass. |
| Least Privilege | Begrenzung des Schadens bei kompromittierten Accounts. |

---

# 11. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Online-/Offline-Cracking | Online: über Login, durch Rate Limits begrenzbar. Offline: mit gestohlenen Hashes lokal und sehr schnell parallelisierbar. |
| Brute Force / Wörterbuchangriff | Brute Force testet systematisch alle Kombinationen; Wörterbuchangriff testet wahrscheinliche Kandidaten. |
| Vollständige Hash-Tabelle / Rainbow Table | Vollständige Tabelle speichert viele Passwort-Hash-Paare; Rainbow Table speichert vor allem Kettenanfänge/-enden und benötigt mehr Rechenzeit. |
| Hash / Salt | Hash ist Prüfergebnis eines Passworts; Salt ist individueller Zusatzwert vor dem Hashen. |
| Salt / Pepper | Salt ist pro Passwort und nicht geheim; Pepper ist systemweit/zusätzlich und muss geheim bleiben. |
| LM Hash / NTLM | LM Hash ist sehr schwaches Legacy-Verfahren mit 7-Zeichen-Teilung; NTLM ist moderner, aber als Passwort-Hash zu schnell. |
| Passwortkomplexität / Passwortentropie | Regeln wie „ein Symbol“ können vorhersehbare Muster erzeugen; tatsächlich zufällige Länge und Vielfalt erhöhen Entropie. |
| Passwort / MFA | MFA ergänzt Passwort durch unabhängigen Faktor, ersetzt aber nicht automatisch sichere Passwortspeicherung. |

---

# 12. Klausur-Checkliste

Du solltest erklären können:

1. Warum Systeme Passwort-Hashes statt Klartext speichern.
2. Online- und Offline-Cracking unterscheiden.
3. Brute Force, Wörterbuchangriff, Keylogger und Social Engineering einordnen.
4. Warum Passwortlänge den Suchraum exponentiell vergrößert.
5. Warum Crackzeiten von Hashverfahren und Hardware abhängen.
6. Vollständige Hash-Tabellen und Time-Memory Trade-off erklären.
7. Rainbow Tables inklusive Hash-/Reduktionsketten erklären.
8. Warum Salt Rainbow Tables stark erschwert.
9. LM Hash und dessen zentrale Schwächen erklären.
10. NTLM und moderne Passwort-Hashverfahren abgrenzen.
11. Warum Argon2/scrypt/bcrypt/PBKDF2 für Passwortspeicherung besser geeignet sind als schnelle Hashfunktionen.
12. Praktische Nutzer- und Betreibermaßnahmen nennen.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Passwords in Windows“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Relevant: Angriffsarten auf Seite 2, Brute-Force-/Tabellenprinzip auf Seiten 3–6, Rainbow Tables auf Seiten 7–14, Maßnahmen auf Seite 15 sowie Passwort-Cracking-/Salt-Einordnung auf Seiten 16–18.
