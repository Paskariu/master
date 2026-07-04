# IT-Sicherheit 5 – Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit 5 – Software-Sicherheit“**.

## Abkürzungen und Register

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ASLR** | Address Space Layout Randomization | Zufällige Anordnung von Speicherbereichen zur Erschwerung gezielter Sprünge. |
| **BSS** | Block Started by Symbol | Segment für nicht initialisierte globale und statische Variablen. |
| **CWE** | Common Weakness Enumeration | Systematik zur Klassifikation allgemeiner Schwachstellentypen. |
| **DEP** | Data Execution Prevention | Kennzeichnet Speicherbereiche als nicht ausführbar. |
| **DWord** | Double Word | 32-Bit-Dateneinheit in der dargestellten x86-Architektur. |
| **EAX** | Accumulator Register | General-Purpose-Register; oft für Rechenoperationen und Rückgabewerte genutzt. |
| **EBP** | Extended Base Pointer | Frame Pointer; Referenzpunkt des aktuellen Stack Frames. |
| **EBX** | Extended Base Register | General-Purpose-Register. |
| **ECX** | Extended Counter Register | General-Purpose-Register, oft Zähler. |
| **EDI** | Extended Destination Index | Zeiger auf Ziel bei Stringoperationen. |
| **EDX** | Extended Data Register | General-Purpose-Register. |
| **EIP** | Extended Instruction Pointer | Adresse des nächsten auszuführenden Befehls. |
| **ESI** | Extended Source Index | Zeiger auf Quelle bei Stringoperationen. |
| **ESP** | Extended Stack Pointer | Zeigt auf obersten aktuellen Stackeintrag. |
| **GCC** | GNU Compiler Collection | Compiler-Suite; im Foliensatz u. a. für Bounds Sanitizer/StackGuard genannt. |
| **LIFO** | Last In, First Out | Speicherprinzip des Stacks: zuletzt abgelegter Wert wird zuerst entnommen. |
| **MMU** | Memory Management Unit | Hardwarekomponente zur Verwaltung/Abbildung virtueller auf physische Adressen. |
| **NOP** | No Operation | Maschinenbefehl ohne fachliche Aktion; klassisch in NOP Sleds. |
| **NX** | No-eXecute | Bezeichnung für nicht ausführbare Speicherbereiche; praktisch DEP. |
| **ROP** | Return-Oriented Programming | Kontrollflussangriff mit vorhandenen kurzen Codefragmenten statt neuem Shellcode. |
| **x86** | — | Prozessorarchitektur/Befehlssatzfamilie; Foliensatz betrachtet 32-Bit-x86. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Buffer** | Speicherbereich fester Größe zum Ablegen von Daten. |
| **Buffer Overflow / Buffer Overrun** | Schreiben/Lesen außerhalb der Grenzen eines Puffers. |
| **Bounds Checking** | Prüfung, ob Array-/Bufferzugriff innerhalb zulässiger Grenzen liegt. |
| **Calling Convention** | Vereinbarung, wie Funktionen Parameter erhalten, Rückgabewerte liefern und Register/Stack verwenden. |
| **Control-Flow Hijacking** | Manipulation des Ausführungsflusses, z. B. über überschriebenes saved EIP. |
| **Data Segment** | Segment für initialisierte globale/statische Variablen. |
| **Data Execution Prevention** | Siehe DEP. |
| **Epilog** | Funktionsende: Stack wiederherstellen, lokalen Speicher freigeben, zurückkehren. |
| **Format String Vulnerability** | Schwachstelle, wenn untrusted Input als Formatstring verarbeitet wird, z. B. `printf(input)`. |
| **Frame Pointer** | Siehe EBP; stabiler Bezugspunkt für Variablen und Parameter einer Funktion. |
| **Frame Pointer Overwrite** | Überschreiben eines gespeicherten Frame Pointers, häufig über Off-by-One-Fehler. |
| **Heap** | Dynamischer Speicherbereich für z. B. `malloc()`-Allokationen; wächst typischerweise zu höheren Adressen. |
| **Heap Overflow** | Überlauf in dynamisch reserviertem Speicher; trifft häufig Nachbardaten/Funktionszeiger. |
| **Heap Spraying** | Massenhaftes Füllen von Heap-Bereichen mit Daten/NOP Sled, um Sprungtreffer wahrscheinlicher zu machen. |
| **Injection Vector** | Weg bzw. Technik, mit der eine Schwachstelle gezielt ausgelöst wird. |
| **Intel-Syntax** | Assemblersyntax, in der meist Ziel vor Quelle steht, z. B. `mov eax, 1`. |
| **Little Endian** | Speicherung mehrbyteiger Werte mit niederwertigstem Byte an niedrigster Adresse. |
| **NOP Sled** | Folge von NOPs vor einem Payload, die Sprungadressgenauigkeit weniger kritisch macht. |
| **Off-by-One Error** | Fehler, bei dem genau ein Element zu viel bzw. zu wenig verarbeitet wird. |
| **Payload** | Aktion oder Programmcode, der nach erfolgreicher Ausnutzung ausgeführt wird. |
| **Pointer** | Variable, die Speicheradresse enthält. |
| **Prolog** | Funktionsbeginn: alten Frame sichern und lokalen Speicher reservieren. |
| **Return Address / Rücksprungadresse** | Adresse, zu der `ret` nach einer Funktionsrückkehr springt; entspricht saved EIP im Frame. |
| **return-to-libc** | Kontrollflussangriff, der statt Shellcode vorhandene Funktionen aus der C-Standardbibliothek aufruft. |
| **saved EBP** | Im Frame gespeicherter vorheriger Frame Pointer. |
| **saved EIP** | Im Frame gespeicherte Rücksprungadresse. |
| **Shellcode** | Kompakter Maschinencode als Payload, historisch oft zum Start einer Shell. |
| **Stack** | LIFO-Speicher für Funktionskontext, lokale Variablen, Parameter und Rücksprungadressen; wächst typischerweise abwärts. |
| **Stack Canary / Security Cookie** | Prüfwert vor Kontrollinformationen, der Stack-Overflows vor `ret` erkennen soll. |
| **Stack Frame** | Zusammenhängender Stackbereich mit Kontext einer laufenden Funktion. |
| **Stack Overflow** | Überlauf eines lokalen Stack-Puffers, der benachbarte Variablen und ggf. saved EIP überschreibt. |
| **Text Segment** | Segment mit auszuführendem Programmcode; typischerweise schreibgeschützt. |
| **Virtual Address Space** | Logischer Speicherbereich eines Prozesses, der durch MMU auf physischen Speicher abgebildet wird. |

## Kritische C-Funktionen und Muster

| Ausdruck | Risiko / Bedeutung |
|---|---|
| `gets(buffer)` | Liest ohne Längenlimit; grundsätzlich unsicher. |
| `strcpy(dest, src)` | Kopiert ohne Kenntnis der Zielpuffergröße; unsicher bei untrusted/zu langem Input. |
| `fgets(buffer, n, stdin)` | Liest begrenzt; sicherer, wenn `n` korrekt ist. |
| `strncpy(dest, src, n)` | Kopiert begrenzt, kann aber bei langem Input ohne Nullterminierung enden. |
| `printf(user_input)` | Format-String-Vulnerability möglich. |
| `printf("%s", user_input)` | Formatstring ist fest; Input wird als Daten behandelt. |
| `%x`, `%p`, `%s` | Können bei Format-String-Fehlern Speicher/Pointer auslesen. |
| `%n` | Schreibt Anzahl ausgegebener Zeichen an übergebene Adresse; bei Fehlern besonders gefährlich. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Stack / Stack Frame** | Stack ist gesamter LIFO-Bereich; Stack Frame ist der Funktionskontext einer einzelnen Funktion. |
| **EBP / ESP** | EBP referenziert stabil den Frame; ESP zeigt dynamisch auf die aktuelle Stackspitze. |
| **saved EBP / saved EIP** | saved EBP stellt alten Frame wieder her; saved EIP bestimmt Ziel der Rückkehr. |
| **Stack Overflow / Heap Overflow** | Stack Overflow kann direkt Rücksprungadresse treffen; Heap Overflow trifft primär Heapdaten/Funktionszeiger. |
| **DEP / ASLR** | DEP verhindert Ausführung aus Datenbereichen; ASLR verschleiert Zieladressen. |
| **Shellcode / ROP** | Shellcode ist neuer eingeschleuster Code; ROP verwendet vorhandene Codefragmente. |
| **Canary / Bounds Checking** | Canary erkennt oft Überschreiben vor Rückkehr; Bounds Checking verhindert/erkennt Zugriff außerhalb eines Arrays. |
| **`strncpy` / vollständig sicher** | `strncpy` ist nur begrenzt sicherer; korrekte Längen- und Nullterminierungsbehandlung bleibt erforderlich. |
