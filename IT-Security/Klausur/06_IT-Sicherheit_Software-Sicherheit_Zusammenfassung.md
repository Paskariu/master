# IT-Sicherheit 5 – Software-Sicherheit

> **Foliensatz:** `5_software_v4g.pdf`  
> **Klausurfokus:** Speicherorganisation und x86-Stack verstehen, Buffer Overflows erklären, Varianten unterscheiden und Gegenmaßnahmen sauber einordnen.  
> **Annahmen der Folien:** 32-Bit-x86, Little Endian, C/C++ und klassisches Prozessspeicherlayout.

---

# 1. Relevanz von Buffer Overflows

Ein **Buffer Overflow** bzw. **Buffer Overrun** liegt vor, wenn mehr Daten in einen Speicherpuffer geschrieben werden, als dieser aufnehmen kann. Nachbarbereiche werden überschrieben.

Warum besonders relevant:

- C und C++ erlauben direkte Speicherzugriffe über Pointer.
- Sie erzwingen keine automatische Grenzprüfung wie etwa Java mit `ArrayIndexOutOfBoundsException`.
- Betriebssysteme, Treiber und viele Anwendungen sind in C/C++ implementiert.
- Angreifer können Daten verändern, Kontrollfluss manipulieren oder eigenen Code ausführen lassen.
- Besonders kritisch sind Prozesse mit hohen Rechten, z. B. Treiber, Systemdienste oder `setuid`-Programme.

Wichtige Klassifikation:

| Kennung | Bedeutung |
|---|---|
| **CWE-119** | Improper Restriction of Operations within the Bounds of a Memory Buffer; allgemeine Buffer-Error-Klasse. |
| **CWE-79** | In den Folien als Buffer Overflow genannt; in der aktuellen CWE-Systematik steht CWE-79 jedoch für Cross-Site Scripting. Für Buffer Overflow ist insbesondere CWE-119 relevant. |

---

# 2. Prozess- und Speicherorganisation

Beim Start einer ausführbaren Datei entsteht ein Prozess mit eigenem **virtuellen Adressraum**. Die **MMU** bildet virtuelle auf physische Adressen ab.

## 2.1 Typisches Speicherlayout eines C-Prozesses

```text
hohe Adressen
+------------------+
| Stack            | wächst nach unten
+------------------+
|                  |
| Heap             | wächst nach oben
+------------------+
| BSS              |
+------------------+
| Data             |
+------------------+
| Text / Code      |
+------------------+
niedrige Adressen
```

| Segment | Inhalt | Eigenschaften |
|---|---|---|
| **Text / Code** | Ausführbare Programmanweisungen. | Typisch read-only gegen Modifikation. |
| **Data** | Initialisierte globale und statische Variablen. | Feste Größe beim Programmstart. |
| **BSS** | Nicht initialisierte globale und statische Variablen. | Feste Größe beim Programmstart. |
| **Heap** | Dynamisch angelegte Datenstrukturen, z. B. über `malloc()`. | Wächst in Richtung höherer Adressen; Freigabe über `free()`. |
| **Stack** | Lokale Variablen, Übergabeparameter, Rücksprungadressen. | Wächst in Richtung niedrigerer Adressen; LIFO-Prinzip. |

## 2.2 Little Endian

Bei **Little Endian** wird das niederwertigste Byte einer mehrbyteigen Zahl an der niedrigsten Adresse gespeichert.

Beispiel für den 32-Bit-Wert `0x11223344`:

```text
Adresse a:   44
Adresse a+1: 33
Adresse a+2: 22
Adresse a+3: 11
```

Relevanz: Rücksprung- und Sprungadressen liegen in dieser Byte-Reihenfolge im Speicher. Das ist bei der Analyse von Speicherfehlern und Exploit-Demonstrationen wichtig.

---

# 3. x86-Register

## 3.1 General-Purpose-Register

| Register | Bedeutung / typische Verwendung |
|---|---|
| **EAX** | Accumulator; arithmetische Operationen, häufig Rückgabewert. |
| **EBX** | Base Register. |
| **ECX** | Counter Register. |
| **EDX** | Data Register. |
| **ESI** | Source Index; Quelle bei String-Operationen. |
| **EDI** | Destination Index; Ziel bei String-Operationen. |

## 3.2 Für Stack und Kontrollfluss zentrale Register

| Register | Bedeutung |
|---|---|
| **ESP** | Stack Pointer; zeigt auf obersten aktuellen Stackeintrag. |
| **EBP** | Base/Frame Pointer; Referenzpunkt für den aktuellen Stack Frame. |
| **EIP** | Instruction Pointer; Adresse des nächsten auszuführenden Befehls. |

---

# 4. Stack und Stack Frame

## 4.1 Stack

Der Stack ist ein **LIFO-Speicher** (*Last In, First Out*).

Er enthält unter anderem:

- lokale Variablen,
- Funktionsparameter,
- Rücksprungadressen,
- vorherige Frame Pointer,
- beim Programmstart auch `argc`, `argv`, `env` sowie Argument- und Umgebungsstrings.

Der Stack wächst bei der betrachteten x86-Konvention Richtung **niedrigerer Adressen**.

## 4.2 Stack Frame

Ein **Stack Frame** ist der zusammenhängende Stack-Bereich einer Funktion.

Er enthält:

- null oder mehr Übergabeparameter,
- lokale Variablen,
- den vorherigen Frame Pointer (**saved EBP**),
- die Rücksprungadresse (**saved EIP**).

Schematisch für eine laufende Funktion:

```text
höhere Adressen
+---------------------------+
| Übergabeparameter          | EBP + 8, EBP + 12, ...
+---------------------------+
| saved EIP / Rücksprungadr. | EBP + 4
+---------------------------+
| saved EBP                 | EBP
+---------------------------+
| lokale Variablen           | EBP - Offset
+---------------------------+
niedrigere Adressen
```

## 4.3 EBP und ESP

| Register | Verhalten |
|---|---|
| **EBP** | Bleibt innerhalb einer Funktion typischerweise konstant und dient zur stabilen Referenzierung. |
| **ESP** | Ändert sich bei `push` und `pop`; zeigt auf die aktuelle Stackspitze. |

Adressierung:

```text
lokale Variable:   EBP - Offset
Parameter:         EBP + 8, EBP + 12, ...
saved EBP:         EBP
saved EIP:         EBP + 4
```

---

# 5. Funktionsaufrufe: Prolog, Epilog und zentrale Befehle

## 5.1 Prolog

Der Funktionsprolog:

1. sichert den vorherigen Frame Pointer,
2. setzt den neuen Frame Pointer,
3. reserviert Speicher für lokale Variablen,
4. richtet ggf. den Stack aus (*ESP alignment*).

Typisch:

```asm
push ebp
mov  ebp, esp
sub  esp, <speicherplatz>
```

## 5.2 Epilog

Der Funktions-Epilog:

1. stellt den vorherigen Stackzustand wieder her,
2. gibt lokalen Speicher frei,
3. kehrt zur aufrufenden Funktion zurück.

Typisch:

```asm
leave
ret
```

`leave` entspricht:

```asm
mov esp, ebp
pop ebp
```

## 5.3 Zentrale Befehle

| Befehl | Wirkung |
|---|---|
| `push WERT` | Dekrementiert `ESP` und schreibt Wert auf Stack. |
| `pop REGISTER` | Liest obersten Stackwert in Register und inkrementiert `ESP`. |
| `call ADRESSE` | Speichert Rücksprungadresse und springt zur Funktion. |
| `ret` | Nimmt Rücksprungadresse vom Stack und setzt damit den Kontrollfluss fort. |
| `mov` | Kopiert Daten. |
| `lea` | Berechnet effektive Adresse, ohne den Speicherinhalt an dieser Adresse zu laden. |

**Klausurpunkt:** `call` speichert konzeptionell die aktuelle Rücksprungadresse; `ret` verwendet die gespeicherte Rücksprungadresse. Genau deshalb ist `saved EIP` bei Stack Overflows sicherheitskritisch.

## 5.4 Intel- vs. AT&T-Syntax

| Intel | AT&T |
|---|---|
| Ziel zuerst: `mov eax, 1` | Quelle zuerst: `movl $1, %eax` |
| Register ohne Präfix | Register mit `%` |
| Immediate ohne Präfix | Immediate mit `$` |
| Speicherzugriff: `[ecx]` | Speicherzugriff: `(%ecx)` |

Die Folien verwenden im weiteren Verlauf Intel-Syntax.

---

# 6. Parameterübergabe

Bei der dargestellten Calling Convention liegen übergebene Parameter oberhalb der Rücksprungadresse im Stack Frame.

Beispiel für `addiere(x, y)`:

```text
EBP + 8  = erster Parameter x
EBP + 12 = zweiter Parameter y
```

Die Rückgabe erfolgt typischerweise über `EAX`.

**Merksatz:**  
Lokale Variablen liegen unterhalb von `EBP`; Parameter oberhalb.

---

# 7. Stack Buffer Overflow

## 7.1 Prinzip

Ein lokaler Puffer auf dem Stack wird ohne ausreichende Längenprüfung beschrieben.

```text
Puffer wird überschrieben
-> benachbarte lokale Variablen werden verändert
-> ggf. saved EBP und saved EIP werden überschrieben
-> Kontrollfluss kann verändert werden
```

Mögliche Folgen:

- Datenmanipulation,
- Umgehung einer Sicherheitsprüfung,
- Programmabsturz,
- Umleitung des Kontrollflusses,
- Ausführung eingeschleusten Maschinen-/Shellcodes.

## 7.2 Typische Ursache

Unsichere Stringoperationen wie:

```c
strcpy(destination, source);
gets(buffer);
```

Beide kennen die Größe des Zielpuffers nicht bzw. prüfen sie nicht ausreichend.

## 7.3 Datenmanipulation statt Codeausführung

Ein Overflow muss nicht sofort die Rücksprungadresse treffen. Schon das Überschreiben einer benachbarten Variablen kann Sicherheitslogik aushebeln.

Beispielidee:

```text
password_buffer wird überlang beschrieben
-> auth_flag liegt im Speicher dahinter
-> auth_flag wird auf einen „wahren“ Wert überschrieben
-> Zugriff wird gewährt
```

Das ist ein wichtiges Prüfungsbeispiel: **Manipulation einer Sicherheitsentscheidung ohne Shellcode**.

## 7.4 Überschreiben von saved EIP

Wenn die Eingabe lang genug ist, kann sie bis zur gespeicherten Rücksprungadresse gelangen:

```text
Puffer -> lokale Variablen -> saved EBP -> saved EIP
```

Beim `ret` übernimmt der Prozessor die manipulierte Adresse als nächsten Ausführungspunkt. Dadurch entsteht eine Kontrollflussübernahme.

---

# 8. Einschleusen und Ausführen von Code: Konzept

## 8.1 Begriffe

| Begriff | Bedeutung |
|---|---|
| **Injection Vector** | Technik, mit der gezielt ein Buffer Overflow ausgelöst wird. |
| **Payload** | Code oder Aktion, die nach erfolgreicher Ausnutzung ausgeführt wird. |
| **Shellcode** | Kompakter Maschinencode, der nach erfolgreicher Kontrollflussübernahme ausgeführt wird, historisch oft zum Start einer Shell. |
| **NOP Sled / NOP-Schlitten** | Folge von `NOP`-Befehlen, um die notwendige Genauigkeit der Sprungadresse zu reduzieren. |

## 8.2 Aufbau eines klassischen Stack-Overflow-Payloads

Konzeptionell:

```text
[NOP Sled][Payload][Füllbytes][überschriebene Rücksprungadresse]
```

Die manipulierte Rücksprungadresse muss nur irgendwo in den NOP Sled zeigen. Die CPU arbeitet dann NOPs ab, bis sie den Payload erreicht.

## 8.3 Warum klassische Shellcodes technisch schwierig sind

- Speicheradresse ist nicht immer bekannt.
- Nullbytes (`0x00`) beenden C-Strings und können den Payload verkürzen.
- Moderne Systeme verhindern häufig Codeausführung aus Datenbereichen.
- Speicheradressen können zufällig angeordnet sein.

---

# 9. Weitere Buffer-Overflow-Varianten

## 9.1 Off-by-One-Fehler

Ein **Off-by-One-Fehler** entsteht, wenn eine Schleife genau ein Element zu viel verarbeitet.

Unsicheres Muster:

```c
for (i = 0; i <= 256; i++)
    buffer[i] = input[i];
```

Für `char buffer[256]` sind nur Indizes `0` bis `255` gültig. Der Zugriff auf `buffer[256]` überschreibt ein zusätzliches Byte.

Folge in Stack-Szenarien:

- Niedrigstes Byte des gespeicherten Frame Pointers kann verändert werden.
- Beim Wiederherstellen des Stack Frames kann dadurch ein kontrollierter Fake-Frame verwendet werden.
- Das ist eine Sonderform des **Frame Pointer Overwrite**.

## 9.2 Heap Overflow

Ein **Heap Overflow** überschreibt Daten in dynamisch reserviertem Speicher.

Unterschied zum Stack Overflow:

| Stack Overflow | Heap Overflow |
|---|---|
| Kann direkt `saved EIP` überschreiben. | `saved EIP` liegt nicht im Heap und wird daher nicht direkt überschrieben. |
| Ziel oft Rücksprungadresse/Kontrollfluss. | Ziel oft benachbarte Daten oder Funktionszeiger. |

Mögliche Folgen eines Heap Overflows:

- Überschreiben benachbarter Variablen,
- Manipulation von Längenwerten oder Berechtigungsflags,
- Manipulation von Funktionszeigern,
- indirekte Kontrollflussübernahme über einen manipulierten Funktionszeiger.

## 9.3 Format-String-Vulnerabilities

Ursache:

```c
printf(user_input);      // unsicher
printf("%s", user_input); // korrekt
```

Wenn Nutzerinput als Formatstring verwendet wird, können Formatparameter interpretiert werden.

| Format | Bedeutung |
|---|---|
| `%d`, `%u` | Ganzzahl |
| `%x`, `%p` | Hexwert bzw. Pointer |
| `%s` | String |
| `%n` | Schreibt Anzahl bereits ausgegebener Zeichen an die übergebene Adresse |

Risiken:

- Offenlegung von Stack-/Speicherinhalten über `%x`, `%p`, `%s`,
- Schreiben in Speicher über `%n`,
- potenziell Manipulation kritischer Werte/Kontrollflussdaten.

**Merksatz:** Nutzereingaben dürfen nie als Formatstring interpretiert werden.

---

# 10. Gegenmaßnahmen: Ursachenbekämpfung

## 10.1 Sichere Programmierung

Unsichere Funktionen vermeiden:

| Unsicher | Besser, aber korrekt verwenden |
|---|---|
| `gets(s)` | `fgets(s, n, stdin)` |
| `strcpy(dest, src)` | `strncpy(dest, src, n)` bzw. moderne sicherere APIs |

Wichtig bei `strncpy`: Die Funktion kann bei zu langem Quellstring ohne Nullterminierung enden. Deshalb muss die Terminierung bewusst geprüft bzw. gesetzt werden.

Weitere Grundregeln:

- Eingabelängen vor Verarbeitung validieren.
- Größen und Offsets zuverlässig prüfen.
- Bei Arrays strikt `< length`, nicht `<= length`.
- Rückgabewerte und Fehlerfälle prüfen.
- Keine untrusted Daten als Formatstring, Kommando oder Pointer verwenden.
- Speicherzugriffe und arithmetische Längenberechnungen auf Über-/Unterläufe prüfen.

## 10.2 Code Review und Analyse

| Maßnahme | Zweck |
|---|---|
| Manueller Source-Code-Audit | Unsichere Konstruktionen erkennen; aufwändig und fehleranfällig. |
| Statische Analyse | Quellcode ohne Ausführung untersuchen, z. B. Flawfinder. |
| Dynamische Analyse / Tracing | Laufzeitfehler und Speicherzugriffe während Ausführung erkennen, z. B. Valgrind oder Microsoft AppVerifier. |
| Automatisierte Tests / Fuzzing | Unerwartete bzw. lange/fehlerhafte Eingaben erzeugen und Fehler finden. |

---

# 11. Gegenmaßnahmen: Schadensbegrenzung

Diese Maßnahmen sind besonders wichtig, wenn Quellcode nicht kurzfristig geändert werden kann. Sie ersetzen keine sichere Programmierung.

## 11.1 Bounds Checking

Compiler- oder Laufzeitmechanismus, der Arraygrenzen überprüft.

Beispiel:

```text
gcc -fsanitize=bounds
```

Vorteil: Fehler werden erkannt; geringer Performanceverlust laut Folien.

## 11.2 Stack Canary / Security Cookie

Ein **Stack Canary** ist ein Prüfwert zwischen lokalen Puffern und Kontrollinformationen wie saved EIP.

Ablauf:

1. Funktion legt Canary im Stack Frame ab.
2. Vor `ret` prüft Funktion, ob Canary unverändert ist.
3. Bei Änderung wird Programm beendet bzw. Fehler ausgelöst.

Ziel: Überschreiben der Rücksprungadresse erkennen.

Beispiele:

- GCC StackGuard
- Microsoft Buffer Security Check (`/GS`)

Grenze: Kann bei Informationslecks oder bestimmten Fehlerarten umgangen werden; schützt nicht alle Speicherfehler.

## 11.3 Libsafe

**Libsafe** ist ein Wrapper für unsichere Bibliotheksfunktionen.

- fängt Aufrufe ab,
- bestimmt den Abstand zwischen Puffer und Rücksprungadresse,
- begrenzt die Anzahl schreibbarer Bytes,
- kann ohne Neukompilierung des Programms eingesetzt werden.

Grenze: Fokus auf Stack-basierte klassische Überläufe; keine vollständige allgemeine Speicherfehlersicherheit.

## 11.4 Non-Executable Stack / DEP

**DEP – Data Execution Prevention** markiert Speicherbereiche als nicht ausführbar.

Ziel: Eingeschleuster Code im Stack/Heap soll nicht direkt ausgeführt werden.

Grenzen/Umgehungsprinzipien:

- Kontrollfluss kann weiterhin verändert werden.
- **return-to-libc**: Sprung zu bereits vorhandenem Code einer Bibliothek.
- **ROP – Return-Oriented Programming**: Kombination vorhandener kleiner Codefragmente (*Gadgets*) über manipulierte Rücksprungadressen.

**Merksatz:** DEP verhindert Code-Injection-Ausführung, aber nicht zwingend Control-Flow Hijacking.

## 11.5 ASLR

**ASLR – Address Space Layout Randomization** ordnet Speicherbereiche zufällig an.

Ziel: Angreifer soll nicht zuverlässig wissen, wo Payload, Bibliotheksfunktionen oder Gadgets liegen.

Umgehungsansätze:

- Informationslecks, die Adressen verraten,
- Brute Force in begrenzten Adressräumen,
- **Heap Spraying**: viele Speicherbereiche mit NOP Sled/Payload füllen, um Trefferwahrscheinlichkeit zu erhöhen.

---

# 12. Zusammenspiel der Gegenmaßnahmen

| Maßnahme | Verhindert primär | Grenze |
|---|---|---|
| Inputvalidierung / sichere APIs | Ursache des Overflows | Fehler in Logik/anderen Pfaden bleiben möglich. |
| Statische/dynamische Analyse | Auffinden von Fehlern | Nicht jeder Fehlerpfad wird gefunden. |
| Stack Canary | Überschreiben von Stack-Kontrollinformationen | Nicht alle Speicherfehler; mögliche Leaks/Umgehungen. |
| DEP / NX | Ausführung neu eingeschleusten Codes | Return-to-libc/ROP bleiben möglich. |
| ASLR | Vorhersagbare Zieladressen | Leaks, Heap Spraying, begrenzte Entropie. |
| Privilegienminimierung | Schadenshöhe nach Exploit | Verhindert Fehler selbst nicht. |
| Kombination | Defense in Depth | Einzelmaßnahmen reichen nicht aus. |

---

# 13. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| **Stack / Heap** | Stack: Funktionskontext, lokale Variablen, Rücksprungadressen; Heap: dynamisch reservierte Daten. |
| **EBP / ESP** | EBP: stabiler Referenzpunkt des Frames; ESP: aktuelle Stackspitze. |
| **saved EBP / saved EIP** | saved EBP: vorheriger Frame Pointer; saved EIP: Rücksprungadresse/Kontrollfluss. |
| **Stack Overflow / Heap Overflow** | Stack Overflow kann direkt Rücksprungadresse treffen; Heap Overflow eher Nachbardaten/Funktionszeiger. |
| **Code Injection / Control-Flow Hijacking** | Code Injection bringt neuen Code ein; Control-Flow Hijacking lenkt Ausführung um, auch zu vorhandenem Code. |
| **DEP / ASLR** | DEP verhindert Ausführung aus Datenbereichen; ASLR verschleiert Adressen. |
| **Canary / Bounds Checking** | Canary erkennt Überlauf vor Rückkehr; Bounds Checking prüft Grenzen beim Zugriff. |
| **`strcpy` / `strncpy`** | `strcpy` kennt kein Ziel-Limit; `strncpy` nimmt ein Limit, kann aber Nullterminierung erfordern. |
| **Format-String Leak / `%n`** | `%x/%p/%s` können Daten auslesen; `%n` kann Speicher schreiben. |

---

# 14. Klausur-Checkliste

Du solltest sicher erklären können:

1. Was ein Buffer Overflow ist und warum C/C++ besonders anfällig sind.
2. Die Segmente Text, Data, BSS, Heap und Stack zuordnen.
3. Warum Heap und Stack in entgegengesetzte Richtungen wachsen.
4. Little Endian anhand eines 32-Bit-Werts erklären.
5. EBP, ESP und EIP unterscheiden.
6. Inhalt eines Stack Frames nennen.
7. Prolog, Epilog, `push`, `pop`, `call`, `ret` und `leave` erklären.
8. Warum lokale Variablen mit `EBP - Offset`, Parameter mit `EBP + Offset` adressiert werden.
9. Wie ein Stack Buffer Overflow Daten, `saved EBP` und `saved EIP` beeinflussen kann.
10. Warum ein Authentifizierungsflag auch ohne Shellcode überschrieben werden kann.
11. Zweck von NOP Sled und Payload erklären.
12. Off-by-One-Fehler und Frame Pointer Overwrite erklären.
13. Stack- und Heap Overflow vergleichen.
14. Format-String-Vulnerability inklusive `printf(user_input)` und `%n` erklären.
15. Ursachenbekämpfung und Schadensbegrenzung unterscheiden.
16. Stack Canary, DEP/NX, ASLR, return-to-libc und ROP einordnen.
17. Begründen, warum Defense in Depth nötig ist.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit 5 – Software-Sicherheit“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Themen: x86-Speicher-/Stackorganisation, Buffer Overflows, Stack/Heap/Format-String-Fehler sowie Ursachenbekämpfung und Schutzmechanismen.
