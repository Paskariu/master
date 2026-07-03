# IT-Sicherheit 0 – Einführung

> **Foliensatz:** `0-einfuehrung_v13b.pdf`  
> **Ziel:** Grundlagen, Begriffe, Schutzziele, Bedrohungen, Schutzmaßnahmen und standardisiertes Schwachstellenmanagement.  
> **Klausurfokus:** Begriffe exakt abgrenzen und Zusammenhänge erklären können.

---

## 1. Grundbegriffe und Abgrenzungen

### 1.1 Was bedeutet „Sicherheit“?
> Fokus der IT-Sicherheit liegt auf Beeinträchtigungen durch bewusste Handlungen (Angriffe) und Schutz vor diesen

Der Begriff ist nicht eindeutig. Man muss immer sagen, **wogegen** geschützt werden soll:

- Bedienfehler und Fahrlässigkeit
- Hardware-Ausfall
- technische bzw. konzeptionelle Fehler
- bewusste, böswillige Handlungen: Angriffe

### 1.2 Safety vs. Security

| Begriff                          | Bedeutung                                                                                                                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Safety / Funktionssicherheit** | Schutz gegen **unbeabsichtigte** Ereignisse, z. B. Ausfall, Defekt, Fehlbedienung oder Programmierfehler. Das System soll seine spezifizierte Soll-Funktion zuverlässig erfüllen. IST = SOLL |
| **Security / IT-Sicherheit**     | Schutz gegen **bewusste, gezielte** Beeinträchtigungen durch Angreifer sowie Schutz vor deren Folgen.                                                                                        |

**Merksatz:**  
Safety = Fehler und Unfälle -> Unabsichtlich
Security = Angriffe und absichtliche Manipulation -> Absichtlich

### 1.3 Datensicherheit, Datensicherung, Datenschutz, Informationssicherheit

| Begriff                     | Definition / Fokus                                                                                                                                                                                                                                         |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Datensicherheit**         | Schutz von Daten gegen Verlust, Manipulation, unbefugte Kenntnisnahme und unberechtigten Zugriff.                                                                                                                                                          |
| **Datensicherung (Backup)** | Schutz gegen **unbeabsichtigten** Datenverlust, z. B. durch Hardwaredefekt, höhere Gewalt oder Fahrlässigkeit. Nicht primär Schutz gegen vorsätzliche Angriffe.                                                                                            |
| **Datenschutz**             | Schutz **personenbezogener Daten** und der betroffenen natürlichen Person vor unrechtmäßiger Verarbeitung.                                                                                                                                                 |
| **Informationssicherheit**  | Vertraulichkeit, Verfügbarkeit, Integrität von Informationen<br><br>Schutz aller schützenswerten Informationen – digital, auf Papier oder als Wissen von Mitarbeitenden – inklusive Management angemessener Maßnahmen, um Beeinträchtigungen zu minimieren |
| **Funktionssicherheit**     | Siehe Safety: Zuverlässigkeit gegenüber Ausfall und unbeabsichtigten Ereignissen.                                                                                                                                                                          |

### 1.4 Datensicherung: typische Maßnahmen

- Backups: z. B. Mirroring, Tape Library, Offsite-Backup.
- Ersatzhardware: Cold Standby, Hot Standby, Ersatzgeräte.
- Ausweichrechenzentrum.
- Redundante Netzanbindung.

**Wichtig:** Ein Backup ist nur dann eine Sicherung, wenn Wiederherstellung tatsächlich funktioniert. Fehlkonfigurierte Backup-Software ist praktisch kein Backup.

### 1.5 Datenschutz

Datenschutz bezieht sich auf **personenbezogene Daten**. Das sind Informationen über eine identifizierte oder identifizierbare natürliche Person.

Beispiele:

- Name, E-Mail-Adresse, Standortdaten
- Gesundheitsdaten
- Online-Identifier / Trackingdaten
- Mitarbeiter- und Kundendaten

Datenschutz umfasst rechtliche **und** technische/organisatorische Schutzmaßnahmen.

### 1.6 Informationssicherheit nach ISO 27000

Informationssicherheit:

- stellt **Vertraulichkeit, Integrität und Verfügbarkeit** von Informationen sicher,
- nutzt und managt angemessene Schutzmaßnahmen,
- berücksichtigt eine breite Menge an Bedrohungen,
- unterstützt kontinuierlichen Geschäftsbetrieb (**Business Continuity**),
- minimiert Beeinträchtigungen durch Sicherheitsvorfälle.

Informationen sind schützenswerte Werte. Sie können digital, materiell oder immateriell vorliegen.

---

## 2. Schutzziele

Die klassischen Schutzziele bilden die **CIA-Triade**:

- **Confidentiality** – Vertraulichkeit
- **Integrity** – Integrität
- **Availability** – Verfügbarkeit

Ergänzende Schutzziele:

- Authentizität
- Verbindlichkeit / Nachweisbarkeit
- Privatheit

## 2.1 Übersicht

| Schutzziel | Kernfrage | Typische Verletzung | Beispiele für Maßnahmen |
|---|---|---|---|
| **Vertraulichkeit** | Wer darf Informationen kennen? | Abhören, Datenleck, unberechtigter Zugriff | Zugriffskontrolle, Verschlüsselung |
| **Verfügbarkeit** | Ist ein Dienst für Berechtigte nutzbar? | DoS, Ausfall, Überlast | Redundanz, Backups, Load Balancing |
| **Integrität** | Sind Daten/Programme vollständig und unverfälscht? | Manipulation, Löschen, Änderung | Berechtigungen, Hashes, Signaturen |
| **Authentizität** | Ist Identität/Herkunft echt? | Spoofing, gestohlene Zugangsdaten | Passwort, Token, Zertifikat, MAC |
| **Verbindlichkeit** | Kann eine Handlung später abgestritten werden? | „Ich habe das nicht versendet.“ | Signatur, beweisbare Logs, Forensik |
| **Privacy** | Behält die Person Kontrolle über ihre Daten? | Tracking, Profilbildung, Zweckänderung | Anonymisierung, Datenschutz, Berechtigungskonzept |

### 2.2 Vertraulichkeit
> Schutz gegen unberechtigte Kenntnisnahme von Informationen.

Geschützt sind nicht nur Inhalte, sondern auch **Metadaten**:

- Kommunikationspartner
- Zeitpunkt
- Ort
- Datenvolumen
- Verbindungsdaten

Eine Verletzung der Vertraulichkeit ist oft nicht direkt sichtbar, etwa bei passivem Abhören.

#### Zugriffskontrolle
> Welches Subjekt darf auf welches Objekt auf welche Weise zugreifen?

Vertraulichkeit verlangt die Festlegung und Kontrolle von Informationsflüssen:

Beispiele: Linux-Rechte `r`, `w`, `x`.

| Modell | Bedeutung |
|---|---|
| **DAC – Discretionary Access Control** | Der Eigentümer eines Objekts entscheidet, wer Zugriff erhält. |
| **RBAC – Role-Based Access Control** | Rechte werden Rollen zugeordnet; Nutzer erhalten Rechte indirekt über ihre Rolle. |
| **MAC – Mandatory Access Control** | Systembestimmte Zugriffskontrolle mit Klassifikationen von Objekten und Freigaben von Subjekten. Nutzer können die Regeln nicht frei ändern. |

#### Warum reine Objektberechtigungen nicht immer reichen

Beispiel aus den Folien:

- Bill darf `Datei_1` lesen und `Datei_2` schreiben.
- Joe darf `Datei_2` lesen.
- Damit kann Bill Informationen aus `Datei_1` in `Datei_2` schreiben.
- Joe kann anschließend die Informationen aus `Datei_1` indirekt lesen.

Das Problem ist ein **unerwünschter Informationsfluss**. Einzelne Lese-/Schreibrechte wirken zunächst korrekt, verhindern aber nicht die Weitergabe der repräsentierten Information.

**Abhilfe:** MAC mit

- **Klassifikation** für Objekte,
- **Freigabe** für Subjekte.

### 2.3 Verfügbarkeit
> Alle Daten sowie Systeme und Ressourcen müssen für autorisierte Nutzer verfügbar und funktionsbereit sein, wenn sie benötigt werden.

Umfasst:
- Hardware und Netzwerk
- Betriebssystem
- Anwendungen
- Speicher und CPUs
- Archive und Sicherungskopien

Verfügbarkeit muss auch bei bestimmten Angriffen zumindest teilweise erhalten bleiben, z. B.:
- Login berechtigter Nutzer trotz Einbruchsversuchen
- Restfunktionalität bei DoS zur Abwehr und Schadensbegrenzung

Schwierig, weil teuer und weil autorisierte und unautorisierte Aktionen bei hoher Last nicht immer sauber unterscheidbar sind.

### 2.4 Integrität
>Sicherstellung der Unversehrtheit und Unverfälschtheit von Daten oder Programmen.

Unterscheidung:

| Begriff | Bedeutung |
|---|---|
| **Datenintegrität** | Daten dürfen nicht unautorisiert oder unbemerkt verändert oder gelöscht werden. |
| **Systemintegrität** | Systeme funktionieren korrekt und ohne beabsichtigte oder unbeabsichtigte Manipulation. |

Änderungen sind nur nach festgelegten Regeln und Berechtigungen zulässig.

Integrität kann wichtiger als Vertraulichkeit sein, z. B. bei:
- Bankdaten
- Klausurnoten
- Logfiles
- Betriebssystemfunktionen

### 2.5 Authentizität
> Sicherstellung der **Echtheit** einer Information oder behaupteten Identität.

| Art                       | Frage                                                          | Mittel                                |
| ------------------------- | -------------------------------------------------------------- | ------------------------------------- |
| **Data Authenticity**     | Stammt die Information wirklich von der angegebenen Quelle?    | Digitale Signatur, MAC                |
| **Entity Authentication** | Ist der Benutzer/das System wirklich die behauptete Identität? | Passwort, Schlüssel, Biometrie, Token |

Voraussetzung ist eine eindeutige Identifikation von Subjekten und Objekten.

**Abgrenzung:** Authentisierung prüft Identität. Autorisierung entscheidet danach über erlaubte Aktionen.

### 2.6 Verbindlichkeit / Nachweisbarkeit
> Nachweis, dass ein Vorgang stattgefunden hat und beteiligte Kommunikationspartner ihn nicht plausibel abstreiten können.

Nachweisbarkeit kann umfassen:
- Identität der Kommunikationspartner
- Erstellen, Senden, Zustellung oder Empfang einer Nachricht
- weitere Umstände: Dauer, Ort, Datenvolumen

Anwendungen:
- Vertragsschluss im E-Commerce
- Abrechnung von Cloud-Rechenzeit
- Nachweis geschäftlicher Handlungen

Mechanismen:
- nachvollziehbare Logfiles
- Forensik
- digitale Signaturen
- **AAA**: Authentication, Authorization, Accounting

### 2.7 Privatheit / Privacy
> Das Individuum soll Kontrolle über persönliche Informationen behalten.

Umfasst:
- Vertraulichkeit von Verbindungsdaten
- Schutz gegen unzulässige Erhebung, z. B. Ortung und Tracking
- Schutz gegen Profilbildung und Datenverknüpfung
- Schutz gegen unzulässige Verarbeitung und Zweckänderung

Schutzmaßnahmen:
- technisch: Anonymisierung, Verschlüsselung
- organisatorisch: Berechtigungskonzepte
- rechtlich: Datenschutzrecht und informationelle Selbstbestimmung

### 2.8 Kryptographie und Schutzziele

| Mechanismus | Unterstützte Schutzziele |
|---|---|
| **Hashfunktion** | Integrität |
| **MAC / Message Authentication Code** | Integrität, Datenauthentizität |
| **Digitale Signatur** | Integrität, Datenauthentizität, Verbindlichkeit |
| **Verschlüsselung** | Vertraulichkeit |

**Prüfungsfalle:** Verschlüsselung allein liefert nicht automatisch Integrität oder Authentizität.

---

## 3. Schwachstellen, Bedrohungen, Risiken und Angriffe

### 3.1 Begriffe

| Begriff                            | Definition                                                                                                              |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Schwachstelle / Weakness**       | Allgemeine prinzipbedingte Schwäche oder ein Punkt, an dem ein System verwundbar werden kann.                           |
| **Verwundbarkeit / Vulnerability** | Konkrete Schwäche, über die Sicherheitsdienste umgangen, getäuscht oder ein System unautorisiert verändert werden kann. |
| **Bedrohung / Threat**             | Potenzial für eine Sicherheitsverletzung durch Ausnutzung einer Schwachstelle.                                          |
| **Asset / Wert**                   | Schützenswertes Gut, z. B. Information, Datenobjekt, Dienst oder System.                                                |
| **Risiko / Risk**                  | Erwarteter Schaden: Wahrscheinlichkeit eines Schadensereignisses × Schadenshöhe.                                        |
| **Angriff / Attack**               | Absichtlicher Versuch, Sicherheitsdienste zu umgehen und die Sicherheitspolitik zu verletzen.                           |
| **Angriffsvektor / Attack Vector** | Methode oder Typ, über den ein Angriff erfolgt.                                                                         |
| **Exploit**                        | Systematische Möglichkeit, meist Programmcode, der eine Verwundbarkeit ausnutzt.                                        |

### 3.2 Weakness vs. Vulnerability

Beispiel Software:
- Allgemeine Schwäche: Software kann Programmierfehler enthalten.
- Konkrete Vulnerability: Buffer Overflow aufgrund fehlender Eingabeprüfung.
- Exploit: Code, der den Buffer Overflow ausnutzt.
- Angriff: Ausführung des Exploits gegen ein System.
- Risiko: Wahrscheinlichkeit und Schadenshöhe für das betroffene Asset.

### 3.3 Risikoformel
> Risikomanagement bedeutet nicht, jedes Risiko zu eliminieren. Es bedeutet, Risiken zu erkennen, zu bewerten und angemessen zu behandeln.

```text
Risiko R = Eintrittswahrscheinlichkeit E × Schadenshöhe S
```

### 3.4 Passive und aktive Angriffe

| Angriffstyp | Ziel | Beispiele |
|---|---|---|
| **Passiv** | Unautorisierte Informationsgewinnung ohne Veränderung | Sniffing, Abhören von Netzwerkverkehr |
| **Aktiv** | Unautorisierte Änderung, Unterbrechung oder Täuschung | Spoofing, Manipulation, Denial of Service |

**Spoofing / Maskierung:** Vorgeben einer falschen Identität, z. B. gefälschte E-Mail-Absenderadresse oder DNS-Antwort.

### 3.5 Cyber Kill Chain

Die Cyber Kill Chain beschreibt typische Phasen eines zielgerichteten Angriffs:

1. **Reconnaissance / Aufklärung**  
   Potenzielle Ziele, Eigenschaften und Schwachstellen identifizieren.
2. **Weaponization / Toolvorbereitung**  
   Angriffswerkzeug vorbereiten, etwa Malware-Dokument oder Trojaner.
3. **Delivery / Einschleusen**  
   Werkzeug in das Zielsystem bringen, z. B. per Mail, Web oder USB.
4. **Exploitation / Ausnutzung**  
   Schwachstelle ausnutzen, um Zugang zu erhalten und sich im System zu bewegen.
5. **Installation**  
   Backdoor oder Malware installieren, Berechtigungen/Konfiguration anpassen.
6. **Command & Control**  
   Verbindung zum C2-Server aufbauen.
7. **Actions on Objectives**  
   Eigentliches Ziel ausführen: Datendiebstahl, Verschlüsselung, Ausbreitung, Sabotage.

**Nutzen für Verteidigung:** Jede Phase bietet mögliche Erkennungs- oder Unterbrechungspunkte.

---

## 4. Angreifer und aktuelle Bedrohungen

### 4.1 Angreifergruppen

| Gruppe | Charakteristik |
|---|---|
| **Innentäter** | Kriminelle, frustrierte oder eingeschleuste Mitarbeitende; häufig hohes Schadenspotenzial durch Systemwissen und vorhandene Berechtigungen. |
| **Hacker / White Hat** | Technisch versiert, sucht und veröffentlicht Schwachstellen, um Aufmerksamkeit und Verbesserungen zu bewirken. |
| **Cracker / Black Hat** | Nutzt technisches Wissen mit krimineller Absicht, z. B. finanzieller Vorteil oder Schädigung. |
| **Script Kiddie** | Geringes technisches Verständnis; nutzt fertige Exploits anderer. |
| **Unternehmen** | Sammeln und analysieren Nutzerdaten, etwa durch Tracking und Telemetrie. |
| **Organisierte Kriminalität** | Phishing, Ransomware, Erpressung. |
| **Cyber-Terroristen** | Störung oder Einschüchterung mit politischem/ideologischem Ziel. |
| **Behörden/Geheimdienste** | Strafverfolgung, Überwachung, Industriespionage, Cyberoperationen. |
| **Staatliche Akteure** | Cyberwar, Spionage, Sabotage, Desinformation. |

### 4.2 Aktuelle Bedrohungskategorien

| Bedrohung                    | Bedeutung                                                                                                       |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Ransomware**               | Lösegeld-Erpressung durch Verschlüsselung von Daten oder Lahmlegen von Systemen.                                |
| **Malware**                  | Schadsoftware, z. B. Viren oder Trojaner.                                                                       |
| **Social Engineering**       | Ausnutzen menschlicher Fehler/Schwächen durch Manipulation, z. B. Phishing.                                     |
| **Data Breach**              | Verletzung des Schutzes personenbezogener Daten, z. B. Verlust, Veränderung oder unbefugte Offenlegung/Zugriff. |
| **(D)DoS**                   | Gezielte Überlastung eines Systems oder Netzwerks.                                                              |
| **Internet Threats**         | Unterbrechungen von Kommunikation etwa durch Naturkatastrophen, Cyber- oder militärische Angriffe.              |
| **Information Manipulation** | Desinformation und Propaganda durch staatliche oder nichtstaatliche Akteure.                                    |
| **Supply-Chain-Angriff**     | Angriff über einen Dritten, z. B. kompromittierte Software-Komponente, Dienstleister oder Entwicklungswerkzeug. |

---

## 5. Schutzmaßnahmen und Sicherheitsinfrastruktur

### 5.1 Definition
> Eine **Schutzmaßnahme** (`countermeasure`, `control`) ist eine Aktion, ein Gerät, eine Prozedur oder Technik, die einer Bedrohung, Schwachstelle oder einem Angriff entgegenwirkt, indem sie:

- ihn verhindert,
- den Schaden reduziert,
- ihn entdeckt und meldet,
- oder Korrekturmaßnahmen ermöglicht.

### 5.2 Strategien für Schutzmaßnahmen

| Strategie    | Bedeutung                            | Beispiel                                     |
| ------------ | ------------------------------------ | -------------------------------------------- |
| **Prevent**  | Angriff verhindern                   | Firewall-Regel, MFA, Patch                   |
| **Deter**    | Angriff erschweren/abschrecken       | sichtbare Überwachung, rechtliche Sanktionen |
| **Deflect**  | Angriff umlenken                     | Honeypot                                     |
| **Detect**   | Angriff erkennen                     | IDS, Logauswertung                           |
| **Recover**  | Wiederherstellung/Schadensbegrenzung | Backups, Disaster Recovery                   |
| **Response** | Auf Vorfall reagieren                | Incident Response, CERT/CSIRT                |

### 5.3 Organisatorische und technische Maßnahmen

| Kategorie | Prävention | Detektion | Reaktion |
|---|---|---|---|
| **Organisatorisch** | Awareness-Kampagnen, Schulungen | Regeln zur Logfile-Auswertung | CERT/CSIRT, Notfallplan |
| **Technisch** | Firewall, Virenscanner | IDS, Virenscanner | IPS, Virenscanner |

**CERT / CSIRT:** Computer Emergency Response Team bzw. Computer Security Incident Response Team.
**IDS**: Intrusion Detection System
**IPS**: Intrustion Prevention System

### 5.4 Defense in Depth / Layering
> Mehrere unterschiedliche/heterogene, sich ergänzende Schutzschichten. Wenn ein Mechanismus versagt, bleibt mindestens ein weiterer wirksam.

**Nicht gemeint**: zwei gleichartige Werkzeuge, z. B. zwei Virenscanner.
**Gemeint**: unterschiedliche Barrieren, die ein Angreifer nacheinander überwinden muss.

#### Beispiele
- Perimeterschutz: Firewall, IDS/IPS, Load Balancing
- Netzwerksegmentierung: Filterung an Übergängen, Notfalltrennung
- Separierung von Diensten: ein Dienst pro physischem/virtuellem Server, Multi-Tier-Architektur
- System Hardening: unnötige Accounts entfernen, restriktive Rechte, Sandboxing
- Patchmanagement
- Host-basiertes IDS und Logging
- Web Application Firewall
- Personal organisatorisches Sicherheitsmanagement, Policies, Awareness
- Disaster Recovery und Business Continuity Planning
- Forensik

### 5.5 Zusammenhang: Asset, Bedrohung, Vulnerability und Control
![[zusammenspiel_threats.png]]
```text
Assets haben Wert.
Bedrohungsakteure wollen Assets missbrauchen oder beschädigen.
Bedrohungen nutzen Vulnerabilities aus.
Vulnerabilities führen zu Risiko.
Owners wollen Risiko minimieren.
Dafür setzen sie Countermeasures ein.
Countermeasures reduzieren Vulnerabilities und/oder die Folgen von Bedrohungen.
```

---

## 6. Schwachstellenmanagement und SCAP

### 6.1 Ziele

Schwachstellenmanagement soll ermöglichen:
- Überblick über viele eingesetzte Systeme behalten
- schnell auf Sicherheitsvorfälle reagieren
- Maßnahmen dokumentieren und Compliance nachweisen
- herstellerübergreifend einheitliche Terminologie und Metriken nutzen
- Gegenmaßnahmen priorisieren

### 6.2 SCAP
> **SCAP – Security Content Automation Protocol** ist eine Sammlung offener Standards für automatisierbares Sicherheitsmanagement.

Einsatz:
- automatische Prüfung auf Verwundbarkeiten
- automatisches Prüfen von Konfigurationen
- Berichtswesen
- Priorisierung von Gegenmaßnahmen

### 6.3 Überblick der SCAP-bezogenen Standards

| Standard  | Bedeutung                                             | Zweck                                                                                     | Ebene         |
| --------- | ----------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------- |
| **CVE**   | Common Vulnerabilities and Exposures                  | Eindeutige Kennung für bekannte konkrete Schwachstellen.                                  | Vulnerability |
| **CVSS**  | Common Vulnerability Scoring System                   | Standardisierte Bewertung der Kritikalität/Schwere einer Schwachstelle.                   | Vulnerability |
| **CPE**   | Common Platform Enumeration                           | Eindeutige standardisierte Bezeichnung von Hardware, OS und Anwendungen.                  | Asset         |
| **OVAL**  | Open Vulnerability and Assessment Language            | XML-Beschreibung von Prüfungen und Konfigurationen, unter denen Schwachstellen auftreten. | Asset         |
| **CWE**   | Common Weakness Enumeration                           | Klassifikation allgemeiner Schwachstellentypen / Ursachen.                                | Vulnerability |
| **CAPEC** | Common Attack Pattern Enumeration and Classification  | Klassifikation bekannter Angriffsvektoren aus Sicht des Angreifers.                       | Threat        |
| **MAEC**  | Malware Attribute Enumeration and Characterization    | Beschreibung von Malware über Fähigkeiten und Verhalten.                                  | Threat        |
| **CCE**   | Common Configuration Enumeration                      | Standardisierte Kennungen für sicherheitsrelevante Konfigurationseinstellungen.           | Asset         |
| **XCCDF** | eXtensible Configuration Checklist Description Format | Strukturierte Beschreibungen von Sicherheits-Checklisten und Konfigurationsprüfungen.     | Asset         |

---

## 7. CVE, CVSS, CPE, OVAL, CWE, CAPEC, MAEC, CCE und XCCDF

### 7.1 CVE – Common Vulnerabilities and Exposures
> Wörterbuch öffentlich bekannter Schwachstellen und Exposures.

Funktion:
- neue Schwachstellen bekommen eindeutige IDs
- Produkte und Tools können Befunde eindeutig referenzieren
- gemeinsame Basis für Austausch zwischen Sicherheitsprodukten, CERTs und Datenbanken

Typischer Bezug: National Vulnerability Database (NVD).

**Abgrenzung:**
- **Vulnerability:** Objekt kann potenziell betroffen werden.
- **Exposure:** Objekt ist bereits einer schädigenden Einwirkung ausgesetzt.

### 7.2 CVSS – Common Vulnerability Scoring System

CVSS bewertet:
- Kritikalität einer Schwachstelle
- Schwierigkeit ihrer Ausnutzung
- Relevanz für die eigene Umgebung
- Priorität für Gegenmaßnahmen

CVSS liefert einen Score von **0 bis 10**. Im Foliensatz wird CVSS v4.0 erwähnt; Vorgänger v3.1, v3.0 und v2.0 sind ebenfalls verbreitet.

**Wichtig:** CVSS ist nicht das vollständige Geschäftsrisiko. Es unterstützt Priorisierung, ersetzt aber keine organisationsspezifische Risikobewertung.

### 7.3 CPE – Common Platform Enumeration
> standardisiertes Namensformat für Komponenten:

```text
cpe:/<part>:<vendor>:<product>:<version>:<update>:<edition>:<language>
```

`part`:
- `h` = Hardware
- `o` = Operating System
- `a` = Application

Beispiele:
```text
cpe:/a:openssl:openssl:0.9.8e:e
cpe:/a:gnu:gzip:1.3.5
cpe:/o:centos:centos:5
cpe:/h:cisco:adaptive_security_appliance:7.1
```

Nutzen: standardisierte Asset-Inventarisierung und Zuordnung von CVEs zu konkret eingesetzten Produkten.

### 7.4 OVAL – Open Vulnerability and Assessment Language

- XML-basierte Beschreibungssprache.
- Beschreibt Systemkonfigurationen, unter denen eine Schwachstelle auftritt.
- Komponenten werden über CPE beschrieben.
- Ermöglicht systemübergreifenden Austausch und automatisierte Prüfungen.

### 7.5 CWE – Common Weakness Enumeration
> Systematik zur Beschreibung und Klassifikation von Schwachstellenarten.

- Beschreibt allgemeine Ursache, nicht einen einzelnen konkreten Produktfehler.
- Beispiel: **CWE-119 Buffer Errors**.

Zusammenhang:
```text
CWE = allgemeiner Fehler-/Schwachstellentyp
CVE = konkrete bekannte Schwachstelle in einem Produkt
```

### 7.6 CAPEC – Common Attack Pattern Enumeration and Classification
> Einheitliches Vokabular für bekannte Angriffsvektoren und Angriffsmuster.

- Perspektive des Angreifers: **„Know your enemy“**.
- Ergänzt CWE und CVE, indem es beschreibt, wie Schwächen praktisch ausgenutzt werden können.

### 7.7 MAEC – Malware Attribute Enumeration and Characterization
> Standardisierte Charakterisierung von Malware.

- Beschreibt Fähigkeiten und Verhaltensmuster.
- Zweck: Informationen über Malware austauschbar und vergleichbar machen.

### 7.8 CCE – Common Configuration Enumeration

- Beschreibt sicherheitsrelevante Konfigurationseinstellungen.
- Eindeutige IDs zur Vereinheitlichung über Tools und Quellen hinweg.

Ein CCE-Eintrag kann enthalten:
- Titel
- Begründung / Discussion
- Prüfschritt / Check
- Korrekturmaßnahme / Fix
- Querverweise auf Standards und Benchmarks

Beispiel aus den Folien:
```text
Disable Password Authentication for SSH
```

CCE kann auf NIST SP 800-53, DISA STIG oder CIS Benchmarks verweisen.

### 7.9 XCCDF
- Format für Checklisten und Konfigurationsprüfungen.
- Nutzt u. a. CCE-Referenzen.
- Hilft, Prüfvorgaben maschinenlesbar, standardisiert und auswertbar zu formulieren.

### 7.10 Beziehungen der Standards

```text
CPE beschreibt das Produkt/Asset.
CVE beschreibt die konkrete Schwachstelle.
CVSS bewertet deren Schwere.
CWE klassifiziert den zugrunde liegenden Schwachstellentyp.
OVAL beschreibt, wie das Vorliegen geprüft werden kann.
CAPEC beschreibt typische Angriffswege.
CCE/XCCDF beschreiben sichere Konfigurationen und Checklisten.
MAEC charakterisiert Malware.
```

---

## 8. Klausur-Checkliste

Du solltest zu diesem Foliensatz Folgendes sauber erklären können:

1. Safety und Security abgrenzen.
2. Datensicherheit, Datensicherung, Datenschutz und Informationssicherheit unterscheiden.
3. Die CIA-Triade und die drei ergänzenden Schutzziele definieren.
4. DAC, RBAC und MAC unterscheiden.
5. Erklären, warum reine Datei-/Objektrechte Informationsflüsse nicht vollständig kontrollieren.
6. Datenintegrität und Systemintegrität abgrenzen.
7. Data Authenticity und Entity Authentication unterscheiden.
8. AAA vollständig auflösen und einordnen.
9. Schwachstelle, Vulnerability, Bedrohung, Risiko, Asset, Angriff, Angriffsvektor und Exploit unterscheiden.
10. Risikoformel erklären.
11. Passive und aktive Angriffe unterscheiden.
12. Die sieben Phasen der Cyber Kill Chain in Reihenfolge nennen.
13. Angreifergruppen und typische Motive einordnen.
14. Ransomware, Malware, Social Engineering, Data Breach, DoS, Information Manipulation und Supply-Chain-Angriffe erklären.
15. Prevent, Deter, Deflect, Detect, Recover und Response unterscheiden.
16. Defense in Depth korrekt erklären: verschiedene Schichten, nicht doppelte gleichartige Tools.
17. Zweck und Komponenten von SCAP nennen.
18. CVE, CVSS, CPE, OVAL, CWE, CAPEC, MAEC, CCE und XCCDF sauber abgrenzen.
19. Den Zusammenhang zwischen CPE, CVE, CVSS, CWE und OVAL erläutern.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit 0. Einführung“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Enthaltene Themen: Grundlagen, Schutzziele, Schwachstellen und Angriffe, Schutzmaßnahmen, Defense in Depth sowie SCAP und Schwachstellenstandardisierung.
