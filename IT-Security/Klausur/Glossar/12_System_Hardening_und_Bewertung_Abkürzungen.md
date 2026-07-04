# IT-Sicherheit – System Hardening und Sicherheitsbewertung: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ACL** | Access Control List | Regelwerk für erlaubte bzw. verbotene Zugriffe. |
| **CC** | Common Criteria | Internationaler Standard zur Bewertung von IT-Sicherheitsprodukten und -systemen. |
| **CCE** | Common Configuration Enumeration | Standardisierte Kennung für sicherheitsrelevante Konfigurationsempfehlungen bzw. -probleme. |
| **CIS** | Center for Internet Security | Non-Profit-Organisation, die u. a. CIS Benchmarks veröffentlicht. |
| **CIS-CAT** | CIS Configuration Assessment Tool | Tool zur Prüfung eines Systems gegen CIS Benchmarks. |
| **CVE** | Common Vulnerabilities and Exposures | Eindeutige Kennung für bekannte konkrete Schwachstellen. |
| **DREAD** | Damage Potential, Reproducibility, Exploitability, Affected Users, Discoverability | Schema zur Risikobewertung im Security Engineering. |
| **DMZ** | Demilitarized Zone | Getrennte Netzwerkzone für öffentlich erreichbare Dienste. |
| **EAL** | Evaluation Assurance Level | Evaluations-/Vertrauenswürdigkeitsstufe in den Common Criteria. |
| **EVG** | Evaluationsgegenstand | Deutsche Bezeichnung für TOE. |
| **FAU** | Security Audit | CC-Funktionsklasse für Sicherheitsprotokollierung und -analyse. |
| **FIA** | Identification and Authentication | CC-Funktionsklasse für Identifikation und Authentisierung. |
| **FIA_UAU** | User Authentication | FIA-Familie für Nutzer-Authentisierung. |
| **GPL** | GNU General Public License | Freie Softwarelizenz. |
| **IC** | Integrated Circuit | Integrierter Schaltkreis, z. B. Chip in Smartcard. |
| **IIS** | Internet Information Services | Webserver-Software von Microsoft. |
| **ISO/IEC 15408** | Common Criteria Standard | Internationale Normenreihe für Common Criteria. |
| **ITSEC** | Information Technology Security Evaluation Criteria | Historische europäische Sicherheitsbewertungskriterien. |
| **MITRE** | MITRE Corporation | Organisation, die u. a. CCE und CVE mitentwickelt bzw. pflegt. |
| **NAT** | Network Address Translation | Übersetzung bzw. Verbergen interner IP-Adressen. |
| **NIST** | National Institute of Standards and Technology | US-Behörde/Institut für Standards, u. a. NVD und CCE-Kontext. |
| **NVD** | National Vulnerability Database | Datenbank für bekannte Schwachstellen und Konfigurationsinformationen. |
| **PP** | Protection Profile | Implementierungsunabhängiges Sicherheitsprofil für eine Produktgruppe. |
| **STRIDE** | Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege | Bedrohungsmodellierungs-Schema. |
| **ST** | Security Target | Konkrete Sicherheitsanforderungen für die Evaluation eines bestimmten TOE. |
| **TCSEC** | Trusted Computer System Evaluation Criteria | Historisches US-Bewertungsmodell, auch Orange Book genannt. |
| **TOE** | Target of Evaluation | Zu evaluierendes Produkt oder System inklusive Dokumentation. |
| **TSP** | TOE Security Policy | Sicherheitsregelwerk innerhalb eines TOE. |
| **TSF** | TOE Security Function | Hardware, Software und Firmware zur Durchsetzung der TSP. |
| **VLAN** | Virtual Local Area Network | Logische Netzwerksegmentierung. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Benchmark** | Vergleichs- bzw. Best-Practice-Vorgabe, gegen die ein System geprüft wird. |
| **Compliance Check** | Prüfung, ob ein System vorgegebenen Sicherheits- oder Konfigurationsanforderungen entspricht. |
| **Defense in Depth** | Mehrere unterschiedliche Schutzschichten statt einer einzelnen Maßnahme. |
| **Hardening** | Systematische Härtung eines Systems durch sichere Konfiguration und Reduktion der Angriffsfläche. |
| **Level 1** | CIS-Profil mit praxistauglichen Maßnahmen, die klaren Nutzen bringen und Usability kaum beeinträchtigen. |
| **Level 2** | Erweiterung von Level 1 für besonders schutzbedürftige Umgebungen; kann Nutzbarkeit/Performance beeinträchtigen. |
| **Not Scored** | CIS-Empfehlung, die keinen Einfluss auf den formalen Benchmark-Score hat. |
| **Protection Profile** | Implementierungsunabhängiges Sicherheitskonzept für eine Produktgruppe. |
| **Rationale** | Begründungsteil eines PP, der den Zusammenhang von Bedrohungen, Zielen und Anforderungen erklärt. |
| **Scored** | CIS-Empfehlung, deren Erfüllung oder Nichterfüllung den Benchmark-Score beeinflusst. |
| **Security Environment** | Beschriebene Einsatzumgebung, Bedrohungen, Annahmen und organisatorischen Vorgaben eines TOE. |
| **Security Functionality** | Sicherheitsfunktionen, die ein Produkt/System bereitstellt. |
| **Security Target** | Konkretes Dokument mit Sicherheitsanforderungen für die Evaluation eines TOE. |
| **Security Assurance** | Vertrauen in Entwicklung, Umsetzung, Tests und Evaluation eines Systems. |
| **Threat Modeling** | Systematische Identifikation und Bewertung von Bedrohungen, z. B. mit STRIDE. |
| **Trustworthiness** | Vertrauenswürdigkeit der Umsetzung und des Entwicklungsprozesses. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **CIS Benchmark / CCE** | Benchmark enthält konkrete Empfehlung und Prüfhinweise; CCE ist deren standardisierte Kennung bzw. Referenz. |
| **CVE / CCE** | CVE = konkrete Schwachstelle; CCE = sicherheitsrelevante Konfiguration. |
| **Level 1 / Level 2** | L1 ist breite Basishärtung; L2 ist strenger und kann Usability/Performance beeinträchtigen. |
| **Scored / Not Scored** | Nur Scored beeinflusst Benchmark-Score. |
| **TOE / ST** | TOE ist das zu bewertende Produkt; ST beschreibt dessen konkrete Sicherheitsanforderungen. |
| **PP / ST** | PP ist allgemein für Produktgruppe; ST ist konkret für einzelnes TOE. |
| **TSP / TSF** | TSP beschreibt Regeln; TSF setzt sie technisch durch. |
| **EAL / reale Produktsicherheit** | EAL beschreibt Evaluations- und Vertrauensniveau, nicht automatisch Sicherheit in jeder Betriebsumgebung. |
| **FIA / FAU** | FIA betrifft Identifikation/Authentisierung; FAU Audit, Logging und Analyse. |
