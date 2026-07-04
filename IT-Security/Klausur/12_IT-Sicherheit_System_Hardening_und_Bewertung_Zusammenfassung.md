# IT-Sicherheit – System Hardening und Sicherheitsbewertung

> **Foliensatz:** `ITsecFR-3.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** CIS Benchmarks, CCE, System Hardening und Common Criteria (CC) inklusive Protection Profiles und EAL.

---

# 1. Überblick

Der Foliensatz behandelt vier eng zusammenhängende Themen:

1. **System Hardening** durch sichere Konfigurationen.
2. **CIS Benchmarks** als Best-Practice-Konfigurationsvorgaben.
3. **CCE** als standardisierte Kennungen für Konfigurationsempfehlungen.
4. **Common Criteria (CC)** zur unabhängigen Bewertung und Zertifizierung von IT-Produkten und -Systemen.

Am Rand werden außerdem genannt:

- Vulnerabilities und CVEs,
- Security Engineering mit STRIDE und DREAD,
- Penetration Testing, z. B. mit OpenVAS.

---

# 2. System Hardening

## 2.1 Definition

**System Hardening** bedeutet, die Angriffsfläche eines Systems durch sichere Konfiguration und Reduktion unnötiger Funktionen zu verkleinern.

Ziele:

- weniger offene Dienste,
- weniger unnötige Rechte,
- weniger Fehlkonfigurationen,
- klare und überprüfbare Sicherheitsvorgaben,
- geringere Wahrscheinlichkeit erfolgreicher Angriffe.

## 2.2 Typische Hardening-Maßnahmen

| Bereich | Beispiele |
|---|---|
| Benutzer und Rechte | Keine Standardkonten, minimale Berechtigungen, starke Passwortregeln. |
| Dienste | Nicht benötigte Dienste, Ports und Features deaktivieren. |
| Betriebssystem | Aktuelle Patches, sichere Konfigurationen, Logging. |
| Netzwerk | Segmentierung, Firewall, VLANs, NAT, DMZ. |
| Anwendungen | Sichere Defaults, Entfernen unnötiger Komponenten, sichere Konfigurationsdateien. |
| Überwachung | Auditing, Monitoring, IDS/IPS, Log-Analyse. |

**Merksatz:** Hardening verhindert nicht jede Schwachstelle, reduziert aber die Zahl erreichbarer und ausnutzbarer Angriffswege.

---

# 3. CIS Benchmarks

## 3.1 CIS

**CIS – Center for Internet Security** ist eine Non-Profit-Organisation mit dem Ziel, Cybersicherheit durch praxisnahe und international anerkannte Empfehlungen zu verbessern.

CIS Benchmarks sind:

- Best-Practice-Empfehlungen für sichere Konfiguration,
- weltweit genutzt,
- de-facto-Standards aus Anwenderperspektive,
- kostenlos als PDF verfügbar,
- für viele Betriebssysteme, Anwendungen, Netzwerkkomponenten und Cloud-Plattformen erhältlich.

Zusätzlich bietet CIS:

- Security Metrics zur Messung des eigenen Sicherheitsstatus,
- **CIS-CAT** zur automatisierten Prüfung von Systemen gegen Benchmarks.

## 3.2 Abgedeckte Systeme

CIS Benchmarks gibt es u. a. für:

| Kategorie | Beispiele |
|---|---|
| Betriebssysteme | Windows, Linux, Apple iOS/macOS, Android, VMware, Xen |
| Netzwerk | Cisco, Check Point |
| Authentisierung | Kerberos, RADIUS |
| Browser | Firefox, Opera, Internet Explorer |
| Datenbanken | MySQL, DB2, Oracle |
| Webserver | IIS, Apache |
| Cloud | Cloud Provider und Cloud-Dienste |
| Anwendungen | Docker, Apache Tomcat usw. |

## 3.3 Aufbau einer Benchmark-Empfehlung

Eine einzelne Empfehlung enthält typischerweise:

| Abschnitt | Inhalt |
|---|---|
| Empfehlung | konkrete Konfiguration, die gesetzt werden soll |
| Profilzuordnung | z. B. Level 1 oder Level 2 |
| Beschreibung | technische Bedeutung der Einstellung |
| Rationale | warum die Maßnahme sicherheitsrelevant ist |
| Audit | wie die Einstellung überprüft wird |
| Remediation | wie sie korrekt gesetzt wird |
| Impact | mögliche Nachteile oder Betriebsfolgen |
| Referenzen | z. B. CCE-Referenz |

## 3.4 Scored vs. Not Scored

| Status | Bedeutung |
|---|---|
| **Scored** | Einhalten erhöht den Benchmark-Score; Nicht-Einhalten senkt ihn. |
| **Not Scored** | Hat keinen Einfluss auf den Benchmark-Score, kann aber trotzdem sicherheitsrelevant sein. |

**Prüfungsfalle:** `Not Scored` bedeutet nicht automatisch „unwichtig“. Es bedeutet nur: kein Einfluss auf den formalen Score.

---

# 4. CIS Security Profiles

## 4.1 Level 1

**Level 1** enthält Maßnahmen, die:

- praktisch und verhältnismäßig sind,
- klaren Sicherheitsgewinn liefern,
- die Nutzbarkeit der Technologie nicht übermäßig beeinträchtigen.

Typischer Einsatz:

- Unternehmensumgebungen,
- allgemeine Server- und Client-Härtung,
- solide Basissicherheit.

## 4.2 Level 2

**Level 2** erweitert Level 1.

Merkmale:

- für Umgebungen mit besonders hohem Schutzbedarf,
- zusätzliche Defense-in-Depth-Maßnahmen,
- kann Nutzbarkeit oder Performance beeinträchtigen.

Typischer Einsatz:

- kritische Systeme,
- besonders sensible Daten,
- hochregulierte oder stark gefährdete Umgebungen.

## 4.3 Vergleich

| Aspekt | Level 1 | Level 2 |
|---|---|---|
| Ziel | pragmatische Basishärtung | maximaler Schutzbedarf |
| Beeinträchtigung | gering | möglicherweise spürbar |
| Einsatz | breite Standardumgebung | besonders schutzbedürftige Systeme |
| Umfang | grundlegende Maßnahmen | Level 1 plus zusätzliche Maßnahmen |

---

# 5. Beispiel: Mindestpasswortlänge

Der Foliensatz verwendet als CIS-Beispiel für Windows 10:

```text
Minimum password length = 14 oder mehr Zeichen
```

Einordnung:

- Level 1,
- Scored,
- für Corporate-/Enterprise-Umgebungen.

## 5.1 Begründung

Die Einstellung soll Schutz gegen:

- Wörterbuchangriffe,
- Brute-Force-Angriffe,
- Offline-Angriffe auf gestohlene Account-/Passwortdatenbanken

verbessern.

Lange **Passphrasen** sind häufig besser als kurze, künstlich komplexe Passwörter:

```text
"I want to drink a $5 milkshake"
```

Die Idee: Eine lange, merkbare Phrase kann mehr mögliche Kombinationen enthalten als ein kurzes Passwort aus zufälligen Zeichen.

## 5.2 Praktische Grenzen

Sehr lange Passwortanforderungen können auch Nachteile erzeugen:

- Nutzer notieren Passwörter unsicher,
- mehr Tippfehler,
- mehr Account-Lockouts,
- mehr Supportaufwand,
- Nutzer wählen vorhersehbare Muster.

Daher:

- Länge ist wichtig,
- Passphrasen und Passwortmanager sind sinnvoll,
- Nutzer müssen geschult werden,
- Sicherheitsregeln müssen verhältnismäßig bleiben.

---

# 6. CCE – Common Configuration Enumeration

## 6.1 Zweck

**CCE – Common Configuration Enumeration** ist ein Standard für eindeutige Kennungen sicherheitsrelevanter Konfigurationseinstellungen.

Die Idee ist ähnlich wie bei CVE:

| Standard | Beschreibt |
|---|---|
| **CVE** | konkrete bekannte Schwachstellen |
| **CCE** | konkrete sicherheitsrelevante Konfigurationsempfehlungen bzw. -probleme |

CCE wurde von MITRE entwickelt und wird im NIST-Kontext veröffentlicht.

## 6.2 Inhalte eines CCE-Eintrags

Ein CCE-Eintrag kann umfassen:

- eindeutige CCE-ID,
- Titel,
- Beschreibung,
- Begründung,
- Audit-Schritte,
- Remediation,
- Referenzen auf Benchmarks oder Standards.

Beispiel aus dem CIS-Foliensatz:

```text
CCE-33789-9
```

bezieht sich auf die Mindestpasswortlänge.

## 6.3 Grenzen

Der Foliensatz weist darauf hin, dass CCE nicht immer zeitnah gepflegt wird. Daher sollte CCE als hilfreiche Referenz, aber nicht als alleinige Quelle für aktuelle Hardening-Entscheidungen verwendet werden.

---

# 7. Sicherheitsbewertung: Ziel und Entwicklung

## 7.1 Ziele einer Sicherheitsbewertung

Sicherheitsbewertung soll ermöglichen:

- einheitliche Bewertungskataloge,
- Vergleichbarkeit von Produkten und Systemen,
- Bewertung von Sicherheitsfunktionen,
- Bewertung der Qualität ihrer Umsetzung,
- unabhängige Evaluation und Zertifizierung,
- Leitlinien für Hersteller und Entwickler,
- Entscheidungshilfe für Anwender und Beschaffung.

## 7.2 Was wird bewertet?

| Aspekt | Bedeutung |
|---|---|
| Maßnahmenkatalog | Welche Sicherheitsgrundfunktionen existieren? |
| Qualität der Realisierung | Wie gut wurde umgesetzt, getestet oder formal verifiziert? |
| Güte der Mechanismen | Wie stark bzw. geeignet sind die verwendeten Sicherheitsmechanismen? |
| Unabhängige Bewertung | Externe Stelle prüft und zertifiziert. |

## 7.3 Historische Entwicklung

| System | Einordnung |
|---|---|
| **Orange Book / TCSEC** | Frühes US-Modell aus den 1980ern; hierarchische Sicherheitsstufen; unzureichende Trennung von Funktion und Umsetzungsqualität. |
| Deutsche IT-Kriterien / Grünbuch | Trennung von Funktionsklassen und Qualitätsstufen. |
| ITSEC | Europäische Kriterien. |
| **Common Criteria** | Internationale Kriterien zur Bewertung von IT-Sicherheit, seit 1996. |

---

# 8. Common Criteria (CC)

## 8.1 Zweck

**Common Criteria (CC)** ist ein internationaler Standard zur Bewertung von IT-Sicherheitsprodukten und -systemen:

```text
ISO/IEC 15408
```

CC bewertet nicht nur „ist das Produkt sicher?“, sondern:

- welche Sicherheitsfunktionen gefordert werden,
- wie diese umgesetzt wurden,
- wie vertrauenswürdig der Entwicklungs- und Evaluationsprozess ist.

## 8.2 Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **TOE – Target of Evaluation** | Produkt oder System inklusive Begleitdokumentation, das bewertet wird. Deutsch: EVG, Evaluationsgegenstand. |
| **ST – Security Target** | Konkrete Sicherheitsanforderungen, die Grundlage der Evaluation eines TOE sind. |
| **PP – Protection Profile** | Implementierungsunabhängige Sicherheitsanforderungen für eine Produktgruppe. |
| **TSP – TOE Security Policy** | Regelwerk, wie Assets im TOE verwaltet, geschützt und verteilt werden. |
| **TSF – TOE Security Function** | Hardware, Software und Firmware zur Durchsetzung der TSP. |
| **EAL – Evaluation Assurance Level** | Stufe der Vertrauenswürdigkeit/Evaluierungstiefe. |

## 8.3 Evaluationsstufen EAL

Es gibt acht EAL-Stufen:

```text
EAL1 bis EAL7
```

Wichtig:

- Höheres EAL bedeutet nicht pauschal „im Alltag sicherer“.
- Höheres EAL beschreibt vor allem höheren Prüf- und Vertrauensaufwand für Entwicklung, Dokumentation, Tests und Evaluation.
- Die konkrete Sicherheitsfunktion und der Einsatzkontext bleiben entscheidend.

Im Foliensatz wird der RFID-Chip des neuen Personalausweises als Beispiel mit **EAL5 hoch** genannt.

---

# 9. CC-Funktionsklassen

## 9.1 Struktur

Funktionale Sicherheitsanforderungen in CC sind hierarchisch aufgebaut:

```text
Klasse
-> Familie
-> Komponente
-> Element
```

## 9.2 Beispiel: FIA

**FIA – Identification and Authentication** umfasst Anforderungen zur Identifikation und Authentisierung.

Beispielstruktur:

```text
FIA
-> FIA_UAU: User Authentication
-> FIA_UAU.3: Fälschungssichere Authentifizierung
-> FIA_UAU.3.2: Verhindert Nutzung kopierter Authentifizierungsdaten
```

## 9.3 Weitere wichtige Klasse: FAU

**FAU – Security Audit** umfasst Anforderungen an:

- Protokollierung sicherheitsrelevanter Ereignisse,
- Analyse von Events,
- Audit-Daten,
- Nachvollziehbarkeit sicherheitsrelevanter Aktionen.

**Merksatz:** FIA betrifft Identifikation/Authentisierung, FAU betrifft Sicherheitsprotokollierung und -analyse.

---

# 10. Protection Profiles (PP)

## 10.1 Zweck

Ein **Protection Profile** beschreibt ein implementierungsunabhängiges Sicherheitskonzept für eine Produktgruppe.

Beispiele:

- Smart Meter,
- Betriebssysteme,
- Netzwerkkomponenten,
- Smartcards.

Nutzen:

| Perspektive | Nutzen |
|---|---|
| Anwender | Bessere Vergleichbarkeit von Produkten, die auf Basis desselben PP entwickelt/evaluiert wurden. |
| Hersteller | PP zeigt, dass ein Sicherheitskonzept marktgerecht und anerkannt ist. |

## 10.2 Aufbau eines Protection Profiles

### 1. Einführung

- eindeutige Identifikation,
- allgemeiner Überblick,
- Einordnung des Schutzprofils.

### 2. EVG-/Produktgruppenbeschreibung

- Beschreibung des TOE bzw. der Produktgruppe,
- Einsatzmöglichkeiten,
- allgemeine Sicherheitseigenschaften,
- Grenzen der Benutzung.

### 3. Sicherheitsumgebung

- geplante Einsatzumgebung,
- erwartete Nutzung,
- Risikoanalyse,
- Bedrohungen,
- Angriffe und Schutzmethoden,
- Bedrohungen, die nicht durch den TOE behandelt werden,
- organisatorische Sicherheitspolitiken,
- Annahmen über sicheren Betrieb.

### 4. Sicherheitsziele

- Sicherheitsziele des TOE,
- Sicherheitsziele der Umgebung,
- wie Bedrohungen behandelt und Policies erfüllt werden.

### 5. IT-Sicherheitsanforderungen

- Anforderungen an Funktionalität,
- Anforderungen an Vertrauenswürdigkeit,
- Bezug auf CC Teil 2 und Teil 3 oder freie Formulierung.

### 6. Optionale PP-Anwendungsbemerkungen

Zusätzliche Informationen für Anwendung oder Umsetzung.

### 7. Erklärungsteil / Rationale

Erklärt, warum Sicherheitsumgebung, Ziele und Anforderungen zusammenpassen:

```text
Bedrohungen
-> Sicherheitsziele
-> Sicherheitsanforderungen
-> wirksames Sicherheitskonzept
```

---

# 11. Bewertung der Vertrauenswürdigkeit

Der Foliensatz beschreibt einen vierstufigen Prozess zur Bewertung der Vertrauenswürdigkeit des Entwicklungsprozesses:

1. Anforderungen,
2. Architekturentwurf,
3. Feinentwurf,
4. Implementierung.

Zusätzlich wird bewertet:

- Wirksamkeit und Zusammenwirken der Funktionalität,
- Stärke der Sicherheitsfunktionen,
- Trennung zwischen Funktionsbewertung und Vertrauenswürdigkeitsbewertung.

Mechanismenstärke wird im Foliensatz beispielsweise als niedrig, mittel oder hoch eingeordnet.

---

# 12. Beispiele und Einordnung

Der Foliensatz nennt unter anderem:

- SUSE Linux Enterprise Server,
- Microsoft Windows 11 und Windows Server 2022,
- Boundary Protection Devices and Systems,
- Smartcards und RFID-Chips,
- Protection Profiles für Betriebssysteme.

Die Tabellen auf den letzten Seiten zeigen, dass viele CC-Zertifizierungen bei Smartcards/ICs, Netzwerk-/Grenzschutzsystemen sowie multifunktionalen Geräten liegen. Sie sollen vor allem die Verbreitung von CC und die Verteilung der EAL-Stufen illustrieren, nicht als Qualitätsrangliste einzelner Hersteller dienen.

---

# 13. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Hardening / Penetration Testing | Hardening reduziert Angriffsfläche präventiv; Pentesting prüft kontrolliert, ob Schwachstellen ausnutzbar sind. |
| CIS Benchmark / CCE | CIS Benchmark = konkrete Best-Practice-Empfehlung; CCE = standardisierte Kennung für Konfigurationsempfehlung/-problem. |
| Scored / Not Scored | Scored beeinflusst Benchmark-Score; Not Scored nicht. |
| Level 1 / Level 2 | Level 1 = alltagstaugliche Basissicherheit; Level 2 = zusätzliche stärkere Maßnahmen mit möglichen Einschränkungen. |
| CVE / CCE | CVE beschreibt konkrete Schwachstelle; CCE beschreibt sicherheitsrelevante Konfiguration. |
| TOE / ST | TOE = zu bewertendes Produkt/System; ST = konkrete Sicherheitsanforderungen für dessen Evaluation. |
| PP / ST | PP = allgemeines, implementierungsunabhängiges Profil für Produktgruppe; ST = konkrete Anforderungen für ein bestimmtes TOE. |
| TSP / TSF | TSP = Sicherheitsregelwerk; TSF = Mechanismen, die dieses Regelwerk durchsetzen. |
| EAL / Produkt-Sicherheitsniveau | EAL beschreibt Tiefe und Vertrauenswürdigkeit der Evaluation, nicht allein die reale Sicherheit in jedem Einsatzszenario. |
| FIA / FAU | FIA = Identifikation und Authentisierung; FAU = Security Audit/Protokollierung. |

---

# 14. Klausur-Checkliste

Du solltest erklären können:

1. Was System Hardening ist und warum es wichtig ist.
2. Ziel und Einsatz von CIS Benchmarks.
3. CIS-CAT, Security Metrics und Compliance Checks einordnen.
4. Scored und Not Scored unterscheiden.
5. Level 1 und Level 2 vergleichen.
6. Das Beispiel „Mindestpasswortlänge 14 Zeichen“ einschließlich Nutzen und Auswirkungen erklären.
7. CCE und CVE abgrenzen.
8. Ziele einer unabhängigen IT-Sicherheitsbewertung nennen.
9. Historische Entwicklung bis Common Criteria grob einordnen.
10. TOE, ST, PP, TSP, TSF und EAL definieren.
11. Warum EAL nicht einfach „Produkt ist sicher“ bedeutet.
12. Die Hierarchie Klasse -> Familie -> Komponente -> Element erklären.
13. FIA und FAU einordnen.
14. Zweck und Aufbau eines Protection Profile erläutern.
15. Bedrohung, Sicherheitsziel und Sicherheitsanforderung im PP logisch verbinden.
16. Hardening, Sicherheitsbewertung und Penetration Testing voneinander abgrenzen.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Themen: CIS Benchmarks, CCE, System Hardening und Common Criteria / ISO 15408.
