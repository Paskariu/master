# IT-Sicherheit – SNORT, IDS und IPS

> **Foliensatz:** `ITsecFR-SNORT.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** IDS/IPS unterscheiden, signatur- und anomaliebasierte Erkennung vergleichen, Snort-Betriebsarten und Regeln erklären, Snorby zur Auswertung einordnen.

---

# 1. Grundidee: Intrusion Detection und Prevention

## 1.1 IDS und IPS

| Begriff | Bedeutung |
|---|---|
| **IDS – Intrusion Detection System** | Erkennt verdächtige Aktivitäten oder Angriffe und erzeugt Meldungen/Logs. |
| **IPS – Intrusion Prevention System** | Erkennt Angriffe und greift aktiv ein, z. B. durch Blockieren oder Verwerfen von Paketen. |

**Merksatz:**  
IDS = erkennen und melden.  
IPS = erkennen und verhindern/blockieren.

Ein IPS kann Schäden reduzieren, birgt aber das Risiko, legitimen Verkehr falsch zu blockieren.

## 1.2 NIDS, HIDS und hybrides IDS

| Typ | Beobachtungsort | Untersucht |
|---|---|---|
| **NIDS – Network IDS** | Netzwerk / Sensor an zentralem Übergang oder Spiegelport | Netzwerkverkehr und Pakete |
| **HIDS – Host IDS** | Einzelner Host | Logs, Dateien, Prozesse, lokale Zustände |
| **Hybrides IDS** | Kombination aus Netz- und Host-Komponenten | Mehrere Datenquellen für bessere Erkennung |

Ein NIDS erkennt Netzwerkangriffe und auffällige Kommunikation. Es sieht aber nur Traffic, der den Sensor tatsächlich passiert. Verschlüsselter Traffic reduziert die Sichtbarkeit des Inhalts.

Ein HIDS kann lokale Ereignisse wie Logins, Systemlogs oder Dateiänderungen sehen, benötigt aber Agenten/Software auf den Hosts.

---

# 2. Erkennungsstrategien

## 2.1 Pattern-/Signaturerkennung

Bei der Pattern-Erkennung wird Verkehr mit bekannten Mustern oder Regeln verglichen.

Beispiele:

- bekannte Exploit-Strings,
- auffällige TCP-Flags,
- bekannte Web-Angriffsmuster,
- typische DNS-Cache-Poisoning-Muster.

### Vorteile

- präzise bei bekannten Angriffen,
- gut nachvollziehbare Meldungen,
- geringer Analyseaufwand bei passenden Regeln.

### Nachteile

- erkennt unbekannte Angriffe, Varianten und Zero Days schlechter,
- Regeln müssen gepflegt und aktualisiert werden,
- Angreifer können Signaturen durch Obfuskation oder kleine Varianten umgehen.

## 2.2 Anomalieerkennung

Anomalieerkennung sucht nach Verhalten, das vom Normalzustand abweicht.

Beispiele:

- ungewöhnlich viele DNS-Antworten,
- atypisches Datenvolumen,
- ungewöhnliche Zielsysteme,
- ungewöhnliche HTTP-Header,
- ungewöhnliche Protokollkombinationen.

Sie benötigt typischerweise eine **Lernphase**, um normales Verhalten zu modellieren.

### Vorteile

- kann unbekannte Angriffe und auffälliges Verhalten erkennen,
- nicht vollständig von bekannten Signaturen abhängig.

### Nachteile

- mehr False Positives,
- Normalverhalten kann sich ändern,
- hohe Anforderungen an Tuning, Kontextwissen und Betrieb.

## 2.3 Vergleich

| Kriterium | Signatur-/Pattern-Erkennung | Anomalieerkennung |
|---|---|---|
| Erkennt bekannte Angriffe | Sehr gut | Möglich, aber indirekt |
| Erkennt unbekannte Angriffe | Schwach | Besser möglich |
| False Positives | Oft geringer bei guten Regeln | Häufig höher |
| Pflege | Regelupdates nötig | Baseline/Tuning nötig |
| Nachvollziehbarkeit | Hoch | Häufig schwieriger |
| Lernphase | Nicht zwingend | Typisch erforderlich |

---

# 3. Snort

## 3.1 Einordnung

**Snort** ist ein Open-Source-Werkzeug für Netzwerküberwachung und Angriffserkennung.

Laut Foliensatz:

- NIDS und je nach Betriebsart NIPS,
- Schwerpunkt auf Pattern-/Signaturerkennung,
- zusätzlich Anomalieerkennung über Preprocessor/Regeln möglich,
- dynamische Regeln,
- benötigt Erfahrung bei Konfiguration und Auswertung,
- seit 2021 Snort 3, neu in C++ implementiert.

## 3.2 Betriebsarten

Snort wird als Command-Line-Tool betrieben und kann mehrere Modi nutzen.

| Modus | Zweck |
|---|---|
| **Sniffer Mode** | Zeigt bzw. analysiert Netzwerkpakete. |
| **Packet Logger Mode** | Zeichnet Netzwerkpakete für spätere Analyse auf. |
| **NIDS Mode** | Prüft Verkehr gegen Regeln und erzeugt Alerts. |
| **Inline Mode** | Snort liegt im aktiven Datenpfad und kann Traffic blockieren. |
| **Passive Mode** | Snort beobachtet Traffic außerhalb des Datenpfads und meldet nur. |

**Wichtig:**  
Passive Platzierung = IDS.  
Inline-Platzierung = IPS/NIPS möglich.

## 3.3 Basissystem und Erweiterungen

Die Architekturfolien zeigen Snort als Kern mit ergänzenden Komponenten:

```text
Netzwerkverkehr
-> Packet Capture / DAQ
-> Preprocessor
-> Detection Engine / Regeln
-> Output / Alerts / Logs
-> Auswertung durch GUI, Datenbank oder SIEM
```

### Typische Bausteine

| Baustein | Aufgabe |
|---|---|
| Packet Capture / DAQ | Erfasst Netzwerkverkehr vom Interface. |
| Preprocessor | Bereitet Traffic auf, dekodiert Protokolle und erkennt bestimmte Anomalien. |
| Detection Engine | Vergleicht Pakete/Flows mit Regeln. |
| Rules | Definieren, wann Alert, Log oder Blockierung erfolgt. |
| Output Plugins | Schreiben Alerts in Dateien, Datenbanken oder externe Systeme. |
| GUI / Reporting | Erleichtert Auswertung, Korrelation und Klassifikation. |

---

# 4. Honeypot-Ergänzung: Glastopf

Die Folie „SNORT + Glastopf“ nennt **Glastopf** als Webserver-Honeypot, der mehr als 1.000 Verwundbarkeiten anbietet.

## Zweck eines Honeypots

Ein Honeypot ist ein absichtlich exponiertes oder kontrolliertes System, das Angriffe anziehen und beobachtbar machen soll.

Mögliche Ziele:

- Angriffsmuster sammeln,
- Scans und Exploitversuche erkennen,
- Signaturen verbessern,
- Angreiferverhalten analysieren,
- reale produktive Systeme entlasten.

## Grenzen

- Ein Honeypot ersetzt keine Absicherung produktiver Systeme.
- Er muss isoliert und überwacht betrieben werden.
- Erkenntnisse sind nicht automatisch repräsentativ für alle Angriffe.

---

# 5. Snorby als GUI

**Snorby** ist eine Oberfläche zur Auswertung von Snort-Ereignissen.

Die Folien zeigen u. a.:

- Dashboard,
- Event-Übersicht,
- Suche,
- Klassifikation,
- Sensoren,
- Signaturen,
- Administration.

## 5.1 Dashboard

Das Dashboard kann Ereignisse nach folgenden Dimensionen darstellen:

- Zeitraum,
- Sensor,
- Schweregrad,
- Protokoll,
- Signatur,
- Quelle,
- Ziel.

Die Abbildungen auf Seiten 11–17 zeigen beispielhaft 330 mittel schwere Events der Signatur `http_inspect: LONG HEADER`; diese lassen sich nach Sensor, Severity, Protocol, Signature, Source und Destination aufschlüsseln.

## 5.2 Event-Klassifikation

Snorby kann Events manuell klassifizieren, etwa als:

- Unauthorized Root Access,
- Unauthorized User Access,
- Attempted Unauthorized Access,
- Denial of Service Attack,
- Policy Violation,
- Reconnaissance,
- Virus Infection,
- False Positive,
- Unclassified.

**Klausurpunkt:** Ein Alert ist keine endgültige Diagnose. Analysten müssen ihn bewerten und klassifizieren.

## 5.3 Event-Details

Ein Event kann enthalten:

| Information | Bedeutung |
|---|---|
| Sensor | Snort-Sensor, der Ereignis erkannt hat. |
| Quelle/Ziel | Source IP und Destination IP. |
| Signatur | Regel bzw. erkannte Angriffsklasse. |
| Severity | Priorität/Schwere der Meldung. |
| IP-/TCP-Header | Technische Paketmetadaten. |
| Payload | Nutzdaten bzw. Ausschnitt der übertragenen Daten. |
| Referenzen | z. B. CVE-Verweise. |
| Generator ID / Signature ID | Identifikatoren für Erzeuger und konkrete Signatur. |

Die Folie auf Seite 19 zeigt u. a. einen Event mit `Generator ID 119`, `Sig ID 19`, einer HTTP-Header-Auffälligkeit und CVE-Referenz.

## 5.4 Suche und Administration

Snorby erlaubt die Suche nach Kriterien wie:

- Quelladresse,
- Zieladresse,
- TCP-/UDP-Quellport,
- TCP-/UDP-Zielport,
- Klassifikation,
- Signatur,
- Signaturname.

Die Administration umfasst u. a.:

- Signaturen,
- Sensoren,
- Schweregrade,
- Benutzer,
- Klassifikationen,
- Reports und Benachrichtigungen.

---

# 6. Snort-Regeln

## 6.1 Grundstruktur

Eine Snort-Regel besteht aus:

1. **Rule Header**
2. **Rule Options**

Beispiel aus den Folien:

```text
alert tcp 1.1.1.1 any -> 2.2.2.2 any
(flags: SF; msg: "SYN-FIN Scan";)
```

## 6.2 Rule Header

Der Header legt fest:

```text
<action> <protocol> <src_ip> <src_port> <direction> <dst_ip> <dst_port>
```

Beispiel:

```text
alert tcp 1.1.1.1 any -> 2.2.2.2 any
```

| Bestandteil | Bedeutung |
|---|---|
| `alert` | Bei Treffer eine Meldung erzeugen. |
| `tcp` | Regel gilt für TCP. |
| `1.1.1.1 any` | Quelle und beliebiger Quellport. |
| `->` | Verkehrsrichtung. |
| `2.2.2.2 any` | Ziel und beliebiger Zielport. |

Mögliche Actions sind je nach Betriebsmodus etwa:

- `alert`
- `log`
- `pass`
- `drop`
- `reject`

`drop` und `reject` sind vor allem im Inline-/IPS-Betrieb relevant.

## 6.3 Rule Options

Options präzisieren die Erkennung.

Typische Optionen im Foliensatz:

| Option | Zweck |
|---|---|
| `flags` | Prüft TCP-Flags. |
| `msg` | Text der Alert-Meldung. |
| `flow` | Richtung und Verbindungszustand, z. B. `to_server`, `to_client`, `established`. |
| `content` | Sucht Byte-/Textmuster im Payload. |
| `depth` | Begrenzung der Suchlänge ab Payload-Beginn. |
| `offset` | Startposition der Suche. |
| `byte_test` | Vergleicht Werte/Bytes im Paket. |
| `detection_filter` | Löst nur bei Häufigkeit innerhalb eines Zeitfensters aus. |
| `metadata` | Zusätzliche Einordnung, etwa Service oder IPS-Policy. |
| `reference` | Verweis auf CVE, Bugtraq, URL usw. |
| `classtype` | Klassifikation des Angriffs. |
| `sid` | Signature ID, eindeutige Regelkennung. |
| `rev` | Revisionsstand der Regel. |

---

# 7. TCP-Flag-Beispiele

Die Folien zeigen drei Scanregeln:

```text
(flags: SF;  msg: "SYN-FIN Scan";)
(flags: S12; msg: "Queso Scan";)
(flags: F;   msg: "FIN Scan";)
```

## Bedeutung

| Regel | Idee |
|---|---|
| SYN-FIN Scan | TCP-Paket mit gleichzeitig gesetztem SYN und FIN ist atypisch und kann auf Scan-/Reconnaissance-Verhalten hindeuten. |
| Queso Scan | Spezifische TCP-Flag-Kombination zur Betriebssystem-/Dienst-Erkennung. |
| FIN Scan | FIN-Pakete können für verdeckte Portscans genutzt werden. |

**Merksatz:** Solche Regeln erkennen nicht zwingend eine erfolgreiche Kompromittierung, sondern oft Reconnaissance oder auffällige Vorstufen eines Angriffs.

---

# 8. Interne Regelrepräsentation

Die Folien 26 und 27 zeigen, dass Snort Regeln intern gruppiert:

- gleicher Header → gemeinsamer **Rule Node**,
- unterschiedliche Bedingungen → mehrere **Option Nodes**.

Beispiel:

```text
Rule Node:
alert tcp 1.1.1.1 any -> 2.2.2.2 any

Option Nodes:
(flags: SF; msg: "SYN-FIN Scan";)
(flags: S12; msg: "Queso Scan";)
(flags: F; msg: "FIN Scan";)
```

Vorteil:

- gemeinsame Paketmerkmale werden nicht für jede Regel vollständig erneut geprüft,
- effizientere Verarbeitung großer Regelmengen,
- Regeln mit gleicher Grundstruktur können zusammengefasst werden.

---

# 9. Beispielregeln aus dem Foliensatz

## 9.1 Heartbleed

Die Heartbleed-Regel prüft eine große TLS-Heartbeat-Response von einem Server.

Wichtige Bestandteile:

```text
alert tcp $HOME_NET [21,25,443,...] -> $EXTERNAL_NET any
```

- Verkehr von internen Servern zu externen Zielen,
- relevante TLS-/SSL-Ports.

```text
flow: to_client, established;
```

- Antwort vom Server in bestehender Verbindung.

```text
content: "|18 03 02|"; depth: 3;
byte_test: 2, >, 128, 0, relative;
```

- erkennt TLS-Heartbeat-Merkmale und prüft, ob die gemeldete Länge auffällig groß ist.

```text
reference: cve, 2014-0160;
classtype: attempted-recon;
sid: 30517;
rev: 8;
```

- verweist auf CVE-2014-0160,
- ordnet Ereignis als möglichen Aufklärungsversuch ein,
- identifiziert Regel und Revisionsstand.

**Inhaltlich:** Heartbleed konnte dazu führen, dass ein Server Speicherbereiche über die erwartete Anfragegröße hinaus zurückgab. Dadurch konnten sensible Daten wie Schlüsselmaterial oder Zugangsdaten offengelegt werden.

## 9.2 Directory Traversal in Nginx

Die Regel für Nginx erkennt einen möglichen Umgehungsversuch über Pfadbestandteile wie:

```text
/../
```

Zentrale Elemente:

```text
flow: to_server, established;
content: "/../"; depth: 40; offset: 5;
metadata: service http;
reference: cve, 2013-4547;
classtype: attempted-user;
```

**Directory Traversal** versucht, durch Pfadmanipulation auf Dateien oder Verzeichnisse außerhalb des vorgesehenen Webroots zuzugreifen.

Beispielidee:

```text
https://server.example/download?file=../../etc/passwd
```

Gegenmaßnahmen:

- Eingaben strikt validieren,
- Pfade kanonisieren,
- Zugriff auf erlaubte Basisverzeichnisse beschränken,
- betroffene Software patchen.

## 9.3 DNS Cache Poisoning

Die DNS-Regel prüft eine große Zahl von NXDOMAIN-Antworten:

```text
alert udp $EXTERNAL_NET 53 -> $HOME_NET any
```

- UDP-Antworten von DNS-Servern an internes Netz.

```text
detection_filter: track by_src, count 1000, seconds 5;
```

- Alarm, wenn eine Quelle 1.000 passende Ereignisse in 5 Sekunden erzeugt.

Weitere `byte_test`-Prüfungen kontrollieren DNS-Flags.

**Zweck:** Erkennung eines auffälligen Musters, das auf DNS-Cache-Poisoning oder vergleichbare Manipulationsversuche hindeuten kann.

---

# 10. Grenzen von Snort und IDS/IPS

| Grenze | Bedeutung |
|---|---|
| Signaturabhängigkeit | Unbekannte Angriffe werden schlechter erkannt. |
| False Positives | Legitime Aktivitäten können Alarm auslösen. |
| False Negatives | Reale Angriffe können unentdeckt bleiben. |
| Verschlüsselung | Payload-Inspektion ist ohne kontrollierte Entschlüsselung eingeschränkt. |
| Sensorposition | Traffic außerhalb des Sichtbereichs wird nicht erkannt. |
| Regelqualität | Schlechte Regeln erzeugen Rauschen oder Lücken. |
| IPS-Risiko | Falsch positive Blockierungen können Verfügbarkeit beeinträchtigen. |
| Betriebsaufwand | Tuning, Updates, Analyse und Incident Response sind notwendig. |

## 10.1 Umgang mit Alerts

Ein sinnvoller Prozess:

1. Alert erfassen.
2. Quelle, Ziel, Protokoll, Signatur und Payload prüfen.
3. Kontext bewerten: legitime Aktivität, Test, Fehlkonfiguration oder Angriff?
4. Event klassifizieren.
5. Bei Bedarf eindämmen: blockieren, Host isolieren, Credentials zurücksetzen.
6. Beweise sichern und nachverfolgen.
7. Regeln und Maßnahmen verbessern.

---

# 11. IDS/IPS im Defense-in-Depth-Modell

Snort ergänzt, ersetzt aber nicht:

| Maßnahme | Ergänzung |
|---|---|
| Firewall | Kontrolliert erlaubte Kommunikation; Snort erkennt auffällige/angriffsartige Muster im erlaubten Verkehr. |
| Endpoint Protection / EDR | Erkennt lokale Malware und Prozessverhalten. |
| Patchmanagement | Entfernt bekannte Schwachstellen, bevor Regeln überhaupt anschlagen müssen. |
| Netzwerksegmentierung | Begrenzung lateraler Bewegung und bessere Sensorplatzierung. |
| TLS / sichere Konfiguration | Schützt Kommunikation; reduziert aber Sichtbarkeit für NIDS-Payload-Analyse. |
| Logging/SIEM | Korrelation von Snort-Alerts mit anderen Sicherheitsereignissen. |
| Incident Response | Reaktion auf bestätigte Vorfälle. |

**Merksatz:** Snort ist ein Detektions- bzw. Präventionsbaustein. Sicherheit entsteht erst durch die Kombination aus Prävention, Erkennung, Reaktion und kontinuierlicher Verbesserung.

---

# 12. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| IDS / IPS | IDS meldet; IPS blockiert oder verwirft aktiv. |
| NIDS / HIDS | NIDS überwacht Netzwerktraffic; HIDS überwacht lokale Hostereignisse. |
| Pattern / Anomalie | Pattern erkennt bekannte Signaturen; Anomalie erkennt Abweichungen vom Normalverhalten. |
| Passive / Inline Mode | Passiv beobachtet und meldet; Inline liegt im Datenpfad und kann blockieren. |
| Sniffer / Packet Logger / NIDS Mode | Sniffer zeigt Traffic; Logger speichert Traffic; NIDS prüft gegen Regeln und alarmiert. |
| Rule Header / Rule Options | Header definiert Verkehrsrahmen; Options definieren genaue Erkennungsbedingungen und Reaktion. |
| `sid` / `rev` | `sid` identifiziert die Regel; `rev` beschreibt ihren Revisionsstand. |
| False Positive / False Negative | False Positive = Fehlalarm; False Negative = unerkannter Angriff. |
| Alert / bestätigter Vorfall | Alert ist Hinweis; Vorfall ist nach Analyse bestätigtes Sicherheitsereignis. |

---

# 13. Klausur-Checkliste

Du solltest erklären können:

1. IDS und IPS sauber unterscheiden.
2. NIDS, HIDS und hybride IDS vergleichen.
3. Pattern-/Signaturerkennung und Anomalieerkennung mit Vor- und Nachteilen erklären.
4. Snort als NIDS/NIPS einordnen.
5. Sniffer Mode, Packet Logger Mode, NIDS Mode, Inline Mode und Passive Mode erklären.
6. Grundarchitektur von Snort mit Packet Capture, Preprocessor, Detection Engine, Rules und Output beschreiben.
7. Honeypot/Glastopf einordnen.
8. Snorby-Dashboard, Event-Klassifikation, Suche und Administration erklären.
9. Aufbau einer Snort-Regel aus Header und Options erklären.
10. `flow`, `content`, `depth`, `offset`, `byte_test`, `detection_filter`, `metadata`, `reference`, `classtype`, `sid` und `rev` einordnen.
11. SYN-FIN-, Queso- und FIN-Scan grob erklären.
12. Heartbleed-, Directory-Traversal- und DNS-Cache-Poisoning-Regel inhaltlich lesen.
13. Interne Gruppierung von Rule Nodes und Option Nodes begründen.
14. Grenzen und Betriebsaufwand eines IDS/IPS nennen.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – SNORT“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Besonders relevant: IDS-/IPS-Grundlagen auf Seiten 2–4, Betriebsmodi auf Seiten 8–9, Snorby-Auswertung auf Seiten 10–24 sowie Rule Engine und Beispielregeln auf Seiten 25–30.
