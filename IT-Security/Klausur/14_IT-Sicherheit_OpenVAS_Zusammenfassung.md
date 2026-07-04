# IT-Sicherheit – OpenVAS und Schwachstellenmanagement

> **Foliensatz:** `ITsecFR-OpenVAS.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** OpenVAS/Greenbone einordnen, Scanablauf und Befunde verstehen, NVT/CVE/CPE/OVAL/CERT erklären sowie CVSS-v2-Basis-, Temporal- und Environmental-Metriken lesen.

---

# 1. Zweck von Schwachstellenscannern

Ein Schwachstellenscanner sucht automatisiert nach bekannten Sicherheitsproblemen, Fehlkonfigurationen, unsicheren Diensten und verwundbaren Produktversionen.

Typische Aufgaben:

- erreichbare Hosts ermitteln,
- offene Ports und laufende Dienste erkennen,
- Betriebssysteme und Produktversionen identifizieren,
- bekannte Schwachstellen prüfen,
- Konfigurationen bewerten,
- Befunde priorisieren und dokumentieren,
- Hinweise auf Patches oder Gegenmaßnahmen geben.

**Wichtig:** Ein Scanbefund ist zunächst ein technischer Hinweis. Er beweist nicht automatisch, dass ein Angreifer das Zielsystem tatsächlich kompromittieren kann.

---

# 2. Nessus, OpenVAS und Greenbone

## 2.1 Nessus

**Nessus** ist ein Schwachstellenscanner.

Laut Foliensatz:

- Nessus wurde 2005 unter eine proprietäre Lizenz gestellt.
- Es untersucht bekannte Schwachstellen.
- Es kann zusätzlich Compliance Checks durchführen.
- Die Folie nennt für Nessus 5.2 rund 55.000 untersuchte Schwachstellen.

## 2.2 OpenVAS

**OpenVAS – Open Vulnerability Assessment System** entstand als freie Weiterentwicklung im GPL-Umfeld.

Eigenschaften laut Foliensatz:

- integriert unter anderem Nmap,
- läuft auf Unix-Systemen,
- Bedienung über Web-Interface,
- erhält Informationen über Feeds, u. a. CERT-Informationen,
- referenziert CVEs,
- kann auf eigenem PC oder in kleinen Netzen betrieben werden.

## 2.3 Greenbone Vulnerability Management

OpenVAS ist heute in **Greenbone Vulnerability Management (GVM)** eingebettet.

| Begriff | Bedeutung |
|---|---|
| **OpenVAS Scanner** | Scan-Komponente, die Schwachstellentests durchführt. |
| **Greenbone / GVM** | Umfassendere Plattform für Asset-, Scan- und Schwachstellenmanagement. |
| **Greenbone Community Edition** | Frei nutzbare Community-Ausgabe mit Community Feed. |
| **Enterprise Feed** | Kommerzieller Feed mit zusätzlicher/anders gepflegter Informationsbasis. |

Die Folien nennen:

- Greenbone als Weiterentwicklung durch Greenbone Networks,
- Version 23.20.1 im Jahr 2025,
- mehr als 120.000 Schwachstelleninformationen zum 1. August 2023,
- über 150.000 Vulnerability Tests und 237.725 CVEs für 2024.

**Klausurpunkt:** OpenVAS ist nicht nur ein Portscanner. Es verbindet Asset-/Dienst-Erkennung mit regelbasierten Schwachstellentests und Informationsquellen.

---

# 3. Grundlegender Scanablauf

Der Scanprozess lässt sich so zusammenfassen:

```text
Ziel definieren
-> Scan-Konfiguration auswählen
-> Hosts/Ports/Dienste erkennen
-> passende NVTs ausführen
-> Befunde mit CVSS bewerten
-> CVE/CPE/OVAL/CERT-Informationen referenzieren
-> Befunde validieren, priorisieren und beheben
-> Nachscan / kontinuierlicher Prozess
```

## 3.1 Ziel und Scan-Task

Die Demo auf den Seiten 6 bis 8 zeigt einen Schnellstart-Scan:

1. IP-Adresse oder Hostname wird eingegeben.
2. Das Tool erzeugt ein Ziel mit vordefinierter Portliste.
3. Es erzeugt eine Scanaufgabe mit vordefinierter Scan-Konfiguration.
4. Der Scan startet.
5. Ergebnisse werden als Berichte dargestellt.

Eine Scanaufgabe enthält typischerweise:

| Element | Bedeutung |
|---|---|
| Ziel | IP-Adresse, Hostname oder Zielnetz. |
| Portliste | Zu prüfende Ports. |
| Scan-Konfiguration | Welche Tests und Intensitäten verwendet werden. |
| Berechtigungen | Wer Scan/Bericht lesen oder verwalten darf. |
| Zeitplan | Zeitpunkt bzw. Wiederholung des Scans. |
| Bericht | Zusammenfassung und Details der Ergebnisse. |

## 3.2 Scanintensität

Die Demo auf Seite 8 zeigt zwei relevante Stellgrößen:

- maximal gleichzeitig ausgeführte NVTs pro Host,
- maximal gleichzeitig gescannte Hosts.

Mehr Parallelität beschleunigt Scans, kann aber:

- Zielsysteme belasten,
- Netzlast erhöhen,
- Fehlalarme oder Timeouts begünstigen,
- produktive Dienste beeinträchtigen.

**Merksatz:** Schwachstellenscans müssen geplant und autorisiert sein. Ein aggressiver Scan ist selbst ein potenzielles Betriebsrisiko.

---

# 4. Ergebnisarten

Die OpenVAS-Oberfläche unterscheidet Befunde nach Schweregrad und Art.

Typische Kategorien:

| Kategorie | Bedeutung |
|---|---|
| **High / Hoch** | Schwerwiegender Befund mit hoher Priorität. |
| **Medium / Mittel** | Mittlere Priorität; Bewertung und Behebung nötig. |
| **Low / Niedrig** | Geringeres Risiko, aber nicht zwingend ignorierbar. |
| **Log** | Informations- oder Erkennungsbefund, z. B. OS-Fingerprinting oder Diensteerkennung. |
| **False Positive** | Vom Scanner gemeldeter Befund, der in der konkreten Umgebung nicht zutrifft. |

Auf Seite 7 werden etwa zwei mittlere Befunde und zahlreiche Log-Einträge dargestellt. Das verdeutlicht:

> Nicht jede Meldung ist eine bestätigte Schwachstelle; Log-Einträge dienen häufig der Inventarisierung oder weiteren Analyse.

---

# 5. Beispiele aus dem Scanbericht

## 5.1 SQL-Injection-Befund

Die Demo zeigt eine **Raritan Power IQ SQL Injection Vulnerability** mit CVSS-Basisscore 6.4 (mittel).

Der Befund enthält:

| Feld | Inhalt |
|---|---|
| Zusammenfassung | Name der erkannten Schwachstelle. |
| Auswirkungen | Angreifer kann SQL-Abfragen manipulieren und Daten lesen, ändern oder löschen. |
| Lösung | Patch des Herstellers installieren. |
| Betroffene Software | Genannte Produktversionen. |
| Schwachstellen-Einblick | Technische Beschreibung der Ursache/Auswirkung. |
| Erkennungsmethode | Im Beispiel zeitbasierte Blind SQL Injection. |
| OID | Kennung des NVT. |

### Blind SQL Injection

Bei **Blind SQL Injection** erhält der Angreifer keine direkte Datenbankausgabe. Stattdessen schließt er aus beobachtbarem Verhalten auf Informationen, etwa:

- unterschiedliche Antwortinhalte,
- Fehler-/Erfolgsverhalten,
- messbare Verzögerungen.

Bei **time-based Blind SQL Injection** wird eine künstliche Wartezeit ausgelöst. Reagiert der Server verzögert, kann dies auf eine wahr ausgewertete Bedingung hindeuten.

**Gegenmaßnahmen:**

- parametrisierte Queries / Prepared Statements,
- serverseitige Eingabevalidierung,
- minimale Datenbankrechte,
- aktuelle Patches,
- sichere Fehlerbehandlung.

## 5.2 Weak SSL Ciphers

Der Scan kann erkennen, ob ein Dienst schwache TLS-/SSL-Verfahren anbietet.

Die Folien klassifizieren u. a. als schwach:

- SSL/TLS ohne Cipher,
- alle SSLv2-Cipher,
- RC4,
- Cipher mit 64 Bit oder weniger,
- RSA-Authentisierung mit 1024 Bit,
- CBC-Cipher in TLS < 1.2 wegen BEAST-/Lucky-13-Risiken.

Die Lösung ist:

```text
Serverkonfiguration ändern, sodass die schwachen Cipher nicht mehr angeboten werden.
```

**Klausurpunkt:** Ein Dienst kann trotz HTTPS/TLS unsicher sein, wenn er veraltete Protokolle oder Cipher Suites anbietet.

## 5.3 POODLE

**POODLE – Padding Oracle On Downgraded Legacy Encryption** ist eine Schwachstelle im Kontext von SSLv3 und CBC Padding.

Die Folien nennen:

- betroffene Software: OpenSSL bis 1.0.1i,
- CVE-2014-3566,
- CVSS-Basisscore 4.3,
- möglicher Man-in-the-Middle-Angriff mit Zugriff auf Klartextdatenstrom,
- einzige korrekte nachhaltige Maßnahme: SSLv3 deaktivieren.

Warum relevant:

- SSLv3 verwendet nichtdeterministisches CBC Padding.
- Padding ist nicht ausreichend durch MAC abgesichert.
- Ein MITM kann mit einem Padding-Oracle-artigen Angriff Klartextinformationen gewinnen.

**Merksatz:** Für POODLE reicht kein kosmetischer Workaround; SSLv3 muss deaktiviert und moderne TLS-Konfiguration verwendet werden.

## 5.4 OS Fingerprinting

Ein OS-Fingerprinting-Test ist oft ein **Log-Befund**, keine Schwachstelle.

Im Beispiel:

- ICMP-basierte Erkennung,
- Ergebnis mit Vertrauenswahrscheinlichkeit,
- Zweck: Betriebssystem bzw. Produktversion des Remote-Hosts bestimmen.

Nutzen:

- Inventarisierung,
- Auswahl passender Schwachstellentests,
- Angriffsflächenanalyse.

Risiko:

- Auch Angreifer können OS-Fingerprinting für Reconnaissance nutzen.

## 5.5 Services-Erkennung

Ein Service-Test versucht zu erkennen, welcher Dienst auf einem Port läuft. Die Folie nennt beispielhaft einen Dienst, der auf Port 80 erreichbar ist, obwohl die Portzuordnung allein nicht zwingend genug für sichere Identifikation ist.

**Wichtig:** Ports liefern Indizien, aber keine vollständige Aussage über den tatsächlich laufenden Dienst. Dienste können auf nicht standardmäßigen Ports betrieben werden.

---

# 6. NVT – Network Vulnerability Test

## 6.1 Definition

Ein **NVT – Network Vulnerability Test** ist ein konkreter Test bzw. eine Prüfregel in OpenVAS/Greenbone.

Ein NVT kann prüfen:

- Produkt-/Versionsinformationen,
- offene Dienste,
- unsichere Konfigurationen,
- bekannte CVEs,
- Webanwendungsprobleme,
- kryptographische Parameter,
- Betriebssystemmerkmale.

NVTs besitzen eigene Kennungen, häufig als **OID – Object Identifier** dargestellt.

## 6.2 Beispielstruktur eines NVT

Die Folien zeigen NVT-Details wie:

| Feld | Bedeutung |
|---|---|
| Name | Bezeichnung des Tests. |
| Familie | Kategorie, z. B. Product Detection oder Web Application Abuses. |
| OID | Globale technische Kennung des Tests. |
| Version | Revision des Testskripts. |
| Zusammenfassung | Was der Test prüft. |
| CVSS | technische Schwerebewertung. |
| Methode | Wie der Test den Befund ermittelt. |
| Verweise | CVEs, Advisories, Herstellerinformationen usw. |

---

# 7. Schwachstelleninformationen und Referenzsysteme

OpenVAS/Greenbone verknüpft Befunde mit standardisierten Informationsquellen.

| Standard/Quelle | Zweck |
|---|---|
| **CVE** | Eindeutige Kennung einer konkreten bekannten Schwachstelle. |
| **CWE** | Klassifikation allgemeiner Schwachstellentypen/Ursachen. |
| **CPE** | Standardisierte Bezeichnung von Produkten, Betriebssystemen und Hardware. |
| **OVAL** | Beschreibung von Prüfungen und Konfigurationen, unter denen Schwachstellen auftreten. |
| **CVSS** | Standardisierte technische Schwerebewertung einer Schwachstelle. |
| **CERT Advisory** | Sicherheitswarnung mit Auswirkungen, betroffenen Produkten und Handlungsempfehlungen. |
| **OID** | Kennung eines NVT bzw. technischen Objekts. |

## 7.1 Beispiel POODLE

Die POODLE-Demo zeigt die Verknüpfung:

```text
NVT
-> CVE-2014-3566
-> CWE-310
-> CVSS 4.3
-> CERT-/Hersteller-Referenzen
```

Das ist der zentrale Nutzen eines Schwachstellenmanagementsystems: Ein technischer Fund wird mit standardisierten Identifikatoren, Schwere, betroffenen Produkten und Maßnahmen verbunden.

---

# 8. CVSS v2

Der Foliensatz erläutert **CVSS – Common Vulnerability Scoring System**, konkret in der CVSS-v2-Notation.

## 8.1 Base Vector

Die Basismetriken bewerten die intrinsischen technischen Eigenschaften einer Schwachstelle:

```text
AV:[L,A,N] / AC:[H,M,L] / Au:[N,S,M] / C:[N,P,C] / I:[N,P,C] / A:[N,P,C]
```

| Metrik | Bedeutung | Werte |
|---|---|---|
| **AV – Access Vector** | Reichweite des Angriffs. | `L` Local, `A` Adjacent Network, `N` Network |
| **AC – Access Complexity** | Komplexität bzw. Voraussetzungen des Angriffs. | `H` High, `M` Medium, `L` Low |
| **Au – Authentication** | Erforderliche Authentisierung zur Ausnutzung. | `N` None, `S` Single, `M` Multiple |
| **C – Confidentiality Impact** | Auswirkung auf Vertraulichkeit. | `N` None, `P` Partial, `C` Complete |
| **I – Integrity Impact** | Auswirkung auf Integrität. | `N` None, `P` Partial, `C` Complete |
| **A – Availability Impact** | Auswirkung auf Verfügbarkeit. | `N` None, `P` Partial, `C` Complete |

### Beispiel aus den Folien

```text
AV:N/AC:M/Au:N/C:P/I:N/A:N
```

Lesart:

- aus dem Netzwerk erreichbar,
- mittlere Angriffskomplexität,
- keine Authentisierung nötig,
- teilweise Auswirkung auf Vertraulichkeit,
- keine Auswirkung auf Integrität,
- keine Auswirkung auf Verfügbarkeit.

## 8.2 Temporal Vector

Der Temporal Vector ergänzt zeitabhängige Faktoren:

| Faktor | Bedeutung |
|---|---|
| **Exploitability** | Wie praktisch ist ein Exploit verfügbar bzw. nutzbar? |
| **Remediation Level** | Wie gut steht eine Gegenmaßnahme/Patch bereit? |
| **Report Confidence** | Wie zuverlässig ist die Schwachstelleninformation? |

## 8.3 Environmental Vector

Der Environmental Vector passt die Bewertung an die konkrete Organisation an:

| Faktor | Bedeutung |
|---|---|
| **Collateral Damage Potential** | Möglicher zusätzlicher Schaden im konkreten Umfeld. |
| **Target Distribution** | Anteil/Verbreitung betroffener Systeme. |
| **C/I/A Requirement** | Bedeutung von Vertraulichkeit, Integrität und Verfügbarkeit für das konkrete Zielsystem. |

**Prüfungsfalle:** Ein CVSS-Basisscore ist kein vollständiger Business-Risk-Score. Erst Environmental-Metriken und organisatorischer Kontext machen eine reale Priorisierung möglich.

---

# 9. SecInfo-Management

OpenVAS/Greenbone verwaltet große Mengen sicherheitsrelevanter Informationen.

Die Folien nennen für verschiedene Stände u. a.:

| Informationstyp | Bedeutung |
|---|---|
| NVTs | konkrete Network Vulnerability Tests |
| CVEs | bekannte konkrete Schwachstellen |
| CPEs | Produkt-/Plattformbezeichnungen |
| OVAL Definitions | maschinenlesbare Prüfdefinitionen |
| DFN-CERT Advisories | Warnmeldungen und Handlungshinweise |

Die Zahlen in den Folien zeigen vor allem die stetig wachsende Menge:

- 2015: 37.577 NVTs, 67.928 CVEs, 194.619 CPEs, 27.098 OVAL-Definitionen und 2.971 DFN-CERT-Advisories.
- 2017: mehr als 50.000 NVTs und 87.924 CVEs.
- 2024: mehr als 150.000 Vulnerability Tests und 237.725 CVEs.

**Klausuridee:** Schwachstellenmanagement ist ein kontinuierlicher Prozess, weil sich Produktlandschaft, Angriffswege und Datenbestände laufend ändern.

---

# 10. Prozess für die Praxis

Ein sinnvoller Ablauf ist:

1. **Asset-Inventar aufbauen**  
   Systeme, Anwendungen, Versionen und Verantwortlichkeiten erfassen.
2. **Scan-Scope definieren**  
   Nur autorisierte Ziele, geeignete Zeitfenster, passende Intensität.
3. **Scannen**  
   NVTs, Dienst-/OS-Erkennung und Konfigurationsprüfungen ausführen.
4. **Validieren**  
   False Positives, Erreichbarkeit, reale Betroffenheit und Ausnutzbarkeit prüfen.
5. **Priorisieren**  
   CVSS plus Business-Kontext, Exponiertheit, Datenwert und Kompensationsmaßnahmen bewerten.
6. **Beheben**  
   Patchen, Konfiguration ändern, Dienst deaktivieren, Netzwerksegmentierung oder zusätzliche Kontrollen einsetzen.
7. **Nachprüfen**  
   Rescan und Wirksamkeitskontrolle.
8. **Dokumentieren**  
   Befunde, Risiken, Maßnahmen, Ausnahmen und Verantwortlichkeiten erfassen.

---

# 11. Grenzen von OpenVAS und Schwachstellenscans

| Grenze | Bedeutung |
|---|---|
| False Positives | Befund trifft im konkreten System nicht zu. |
| False Negatives | Bestehende Schwachstelle wird nicht gefunden. |
| Versions-/Banner-Erkennung | Version kann falsch erkannt oder absichtlich verborgen sein. |
| Kontext | Scanner kennt Geschäftsrelevanz, Kompensationsmaßnahmen und reale Datenwerte nicht vollständig. |
| Neue Zero Days | Kein Test verfügbar, solange Schwachstelle unbekannt ist. |
| Betriebsrisiko | Aggressive Prüfungen können Systeme belasten oder stören. |
| Keine vollständige Sicherheit | Scanner ergänzt, ersetzt aber nicht Penetration Testing, Code Reviews, Hardening, Monitoring und Patchmanagement. |

---

# 12. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Nmap / OpenVAS | Nmap fokussiert Host-, Port- und Diensterkennung; OpenVAS ergänzt umfassende Schwachstellen- und Konfigurationsprüfungen. |
| OpenVAS / GVM | OpenVAS ist die Scannerkomponente; GVM ist die umfassende Managementplattform. |
| NVT / CVE | NVT ist eine konkrete Prüfregel; CVE bezeichnet eine bekannte konkrete Schwachstelle. |
| CVE / CWE | CVE = konkreter Produktfehler; CWE = allgemeine Fehlerklasse/Ursache. |
| CVSS / Business Risk | CVSS bewertet technische Schwere; Business Risk berücksichtigt zusätzlich Geschäftskontext. |
| Log-Befund / Vulnerability | Log-Befund liefert Information, z. B. OS oder Dienst; Vulnerability-Befund zeigt potenzielles Sicherheitsproblem. |
| False Positive / False Negative | False Positive = fälschlich gemeldet; False Negative = vorhandenes Problem nicht erkannt. |
| Scan / Penetration Test | Scan ist automatisierte Befundsuche; Pentest prüft gezielt und autorisiert Ausnutzbarkeit sowie Auswirkungen. |
| CPE / OVAL | CPE benennt ein Produkt; OVAL beschreibt Prüfregeln bzw. Zustände. |

---

# 13. Klausur-Checkliste

Du solltest erklären können:

1. Nessus, OpenVAS und Greenbone/GVM korrekt einordnen.
2. Warum ein Scanfund keine bestätigte Kompromittierbarkeit beweist.
3. Die Schritte Ziel, Portliste, Scan-Konfiguration, Task und Bericht erklären.
4. Warum Scanintensität und Parallelität sorgfältig gewählt werden müssen.
5. High/Medium/Low/Log/False Positive unterscheiden.
6. SQL Injection, weak SSL ciphers, POODLE, OS Fingerprinting und Services-Erkennung im Scanbericht einordnen.
7. Was ein NVT ist und welche Informationen seine Details enthalten.
8. CVE, CWE, CPE, OVAL, CVSS, CERT Advisory und OID abgrenzen.
9. CVSS-v2-Base-Vector vollständig erklären.
10. Temporal und Environmental Vectors erklären.
11. Weshalb ein Basisscore keine vollständige Risikoentscheidung ersetzt.
12. Den kontinuierlichen Schwachstellenmanagementprozess beschreiben.
13. Grenzen automatisierter Scanner nennen.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – OpenVAS“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Besonders relevant: OpenVAS/Greenbone-Einordnung auf Seiten 2–3, Scan- und Befundbeispiele auf Seiten 6–20, CVSS auf Seiten 21–23 sowie SecInfo-Management auf Seiten 25–26.
