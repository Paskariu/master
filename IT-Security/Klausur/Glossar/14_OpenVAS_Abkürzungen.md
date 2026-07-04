# IT-Sicherheit – OpenVAS: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – OpenVAS“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **AC** | Access Complexity | CVSS-v2-Metrik für Komplexität/Voraussetzungen eines Angriffs. |
| **Au** | Authentication | CVSS-v2-Metrik für erforderliche Authentisierung zur Ausnutzung. |
| **AV** | Access Vector | CVSS-v2-Metrik für Reichweite bzw. Angriffspfad. |
| **C** | Confidentiality Impact | CVSS-v2-Metrik für Auswirkung auf Vertraulichkeit. |
| **CBC** | Cipher Block Chaining | Blockchiffre-Betriebsmodus; bei POODLE/SSLv3 relevant. |
| **CERT** | Computer Emergency Response Team | Quelle für Sicherheitswarnungen und Advisories. |
| **CPE** | Common Platform Enumeration | Standardisierte Bezeichnung von Hardware, Betriebssystemen und Anwendungen. |
| **CVE** | Common Vulnerabilities and Exposures | Eindeutige Kennung einer konkreten bekannten Schwachstelle. |
| **CVSS** | Common Vulnerability Scoring System | Standard zur technischen Bewertung der Schwere einer Schwachstelle. |
| **CWE** | Common Weakness Enumeration | Klassifikation allgemeiner Schwachstellenarten und Ursachen. |
| **DFN** | Deutsches Forschungsnetz | Wissenschaftsnetz; im Foliensatz über DFN-CERT Advisories referenziert. |
| **GVM** | Greenbone Vulnerability Management | Plattform für Asset-, Scan- und Schwachstellenmanagement. |
| **GPL** | GNU General Public License | Freie Softwarelizenz. |
| **ICMP** | Internet Control Message Protocol | Netzwerkprotokoll; im Foliensatz für OS Fingerprinting verwendet. |
| **MITM** | Man-in-the-Middle | Angreifer sitzt zwischen Kommunikationspartnern und kann Kommunikation beeinflussen. |
| **NVT** | Network Vulnerability Test | Konkreter OpenVAS-/Greenbone-Test für eine Schwachstelle, Konfiguration oder Erkennung. |
| **OID** | Object Identifier | Globale technische Kennung, etwa für einen NVT. |
| **OS** | Operating System | Betriebssystem. |
| **OVAL** | Open Vulnerability and Assessment Language | Beschreibungssprache für Schwachstellen- und Konfigurationsprüfungen. |
| **POODLE** | Padding Oracle On Downgraded Legacy Encryption | Angriff auf SSLv3/CBC-Padding, der Klartextinformationen offenlegen kann. |
| **RC4** | Rivest Cipher 4 | Veraltete Stromchiffre, die als schwach gilt. |
| **RSA** | Rivest-Shamir-Adleman | Asymmetrisches Kryptosystem; 1024-Bit-RSA wird in den Folien als zu schwach eingeordnet. |
| **SQL** | Structured Query Language | Sprache für relationale Datenbanken. |
| **SSL** | Secure Sockets Layer | Veraltete Vorläuferprotokollfamilie von TLS. |
| **TLS** | Transport Layer Security | Protokoll für kryptographisch geschützte Client-Server-Kommunikation. |

## CVSS-v2-Werte

| Kürzel | Bedeutung |
|---|---|
| **AV:L / AV:A / AV:N** | Local / Adjacent Network / Network |
| **AC:H / AC:M / AC:L** | High / Medium / Low |
| **Au:N / Au:S / Au:M** | None / Single / Multiple |
| **C:N / C:P / C:C** | None / Partial / Complete |
| **I:N / I:P / I:C** | None / Partial / Complete |
| **A:N / A:P / A:C** | None / Partial / Complete |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Asset-Inventar** | Erfasste Liste der Systeme, Anwendungen, Versionen und Verantwortlichkeiten einer Organisation. |
| **Base Vector** | CVSS-Metriken für die grundlegenden, zeitunabhängigen technischen Eigenschaften einer Schwachstelle. |
| **Compliance Check** | Prüfung gegen Konfigurations- oder Sicherheitsanforderungen. |
| **Environmental Vector** | CVSS-Metriken zur Anpassung der Bewertung an das konkrete Zielsystem und Organisationsumfeld. |
| **False Negative** | Vorhandene Schwachstelle wird vom Scanner nicht erkannt. |
| **False Positive** | Scanner meldet ein Problem, das im konkreten Kontext nicht zutrifft. |
| **Greenbone Community Feed** | Community-Datenquelle mit NVTs und Schwachstelleninformationen. |
| **Log-Befund** | Informationsbefund ohne unmittelbar bewertete Schwachstelle, z. B. Dienste- oder OS-Erkennung. |
| **OS Fingerprinting** | Ermittlung des Betriebssystems anhand beobachtbarer Netzwerkmerkmale. |
| **Penetration Test** | Autorisierter, gezielter Test der tatsächlichen Ausnutzbarkeit und Auswirkungen von Schwachstellen. |
| **Portliste** | Zusammenstellung der Ports, die ein Scan prüfen soll. |
| **Remediation** | Gegenmaßnahme zur Behebung oder Minderung eines Befunds, z. B. Patch oder Konfigurationsänderung. |
| **Scan-Konfiguration** | Festlegung, welche NVTs, Intensitäten und Optionen ein Scan verwendet. |
| **SecInfo Management** | Verwaltung und Verknüpfung von Sicherheitsinformationen wie NVT, CVE, CPE, OVAL und Advisories. |
| **Service Detection** | Erkennung des tatsächlich laufenden Dienstes auf einem Port. |
| **Temporal Vector** | CVSS-Metriken, die zeitliche Faktoren wie Exploit-Verfügbarkeit und Patchstatus berücksichtigen. |
| **Time-based Blind SQL Injection** | Blind-SQLi, bei der Verzögerungen als Informationskanal genutzt werden. |
| **Vulnerability Management** | Kontinuierlicher Prozess aus Inventarisierung, Scan, Bewertung, Priorisierung, Behebung und Nachprüfung. |
| **Weak Cipher** | Kryptographisch schwaches bzw. veraltetes Verschlüsselungsverfahren oder Protokoll. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Nmap / OpenVAS** | Nmap dient v. a. zur Host-/Port-/Diensterkennung; OpenVAS führt zusätzlich viele konkrete Schwachstellentests aus. |
| **OpenVAS / GVM** | OpenVAS ist Scannerkomponente; GVM ist die umfassende Greenbone-Managementplattform. |
| **NVT / CVE** | NVT ist ein Testskript; CVE ist Kennung einer konkreten Schwachstelle. |
| **CVE / CWE** | CVE beschreibt einzelnen Produktfehler; CWE beschreibt allgemeine Fehlerklasse. |
| **CPE / OVAL** | CPE beschreibt Produkt/Plattform; OVAL beschreibt Prüf- und Konfigurationsdefinitionen. |
| **CVSS / Business Risk** | CVSS bewertet technische Schwere; Business Risk berücksichtigt zusätzlich organisatorische Bedeutung und Folgen. |
| **Log-Befund / Schwachstellenbefund** | Log liefert Erkennungsinformation; Schwachstellenbefund zeigt ein potenzielles Sicherheitsproblem. |
| **False Positive / False Negative** | False Positive = Fehlalarm; False Negative = nicht erkannte reale Schwachstelle. |
| **Scan / Penetration Test** | Scan automatisiert Hinweise; Pentest bewertet gezielt reale Ausnutzbarkeit und Auswirkungen. |
