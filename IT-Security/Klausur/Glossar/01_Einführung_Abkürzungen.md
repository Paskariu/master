# IT-Sicherheit 0 – Abkürzungen und Bedeutungen

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit 0 – Einführung“**.

| Abkürzung | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **AAA** | Authentication, Authorization, Accounting | Infrastruktur für Authentisierung, Berechtigungsprüfung und nachvollziehbare Abrechnung/Protokollierung. |
| **Asset** | — | Schützenswertes Gut, z. B. Information, Datenobjekt, Dienst oder System. Kein Akronym, aber zentraler Fachbegriff. |
| **CAPEC** | Common Attack Pattern Enumeration and Classification | Standardisierte Sammlung und Klassifikation bekannter Angriffsmuster. |
| **CCE** | Common Configuration Enumeration | Standardisierte Kennungen für sicherheitsrelevante Konfigurationseinstellungen. |
| **CERT** | Computer Emergency Response Team | Team zur Behandlung und Koordination von Sicherheitsvorfällen. |
| **CIA** | Confidentiality, Integrity, Availability | Die drei klassischen Schutzziele: Vertraulichkeit, Integrität und Verfügbarkeit. |
| **CPE** | Common Platform Enumeration | Standardisiertes Namensformat für Hardware, Betriebssysteme und Anwendungen. |
| **CSIRT** | Computer Security Incident Response Team | Bezeichnung für ein Team zur Reaktion auf IT-Sicherheitsvorfälle; praktisch ähnlich zu CERT. |
| **CVE** | Common Vulnerabilities and Exposures | Eindeutige Kennung für eine konkrete öffentlich bekannte Schwachstelle oder Exposure. |
| **CVSS** | Common Vulnerability Scoring System | Standard zur Bewertung der Schwere und Ausnutzbarkeit von Schwachstellen, üblicherweise als Score von 0 bis 10. |
| **CWE** | Common Weakness Enumeration | Klassifikation allgemeiner Schwachstellentypen bzw. Ursachen. |
| **DAC** | Discretionary Access Control | Zugriffskontrolle, bei der der Eigentümer eines Objekts über Zugriffe entscheidet. |
| **DoS** | Denial of Service | Gezielte Beeinträchtigung der Verfügbarkeit eines Dienstes oder Netzwerks. |
| **DS-GVO** | Datenschutz-Grundverordnung | EU-Verordnung zum Schutz personenbezogener Daten. |
| **IDS** | Intrusion Detection System | System zur Erkennung von Angriffen oder verdächtigen Aktivitäten. |
| **IPS** | Intrusion Prevention System | System, das Angriffe nicht nur erkennt, sondern aktiv blockieren oder verhindern kann. |
| **MAC** | Message Authentication Code | Kryptographischer Prüfwert mit gemeinsamem geheimen Schlüssel; sichert Integrität und Datenauthentizität. |
| **MAEC** | Malware Attribute Enumeration and Characterization | Standardisierte Beschreibung von Eigenschaften, Fähigkeiten und Verhalten von Malware. |
| **NVD** | National Vulnerability Database | Datenbank für öffentlich bekannte Schwachstellen, u. a. mit CVE-Bezug. |
| **OVAL** | Open Vulnerability and Assessment Language | XML-basierte Sprache zur Beschreibung von Konfigurationen und Schwachstellenprüfungen. |
| **RBAC** | Role-Based Access Control | Zugriffskontrolle über Rollen: Rechte werden Rollen zugeordnet, Nutzer erhalten sie über ihre Rolle. |
| **RFC** | Request for Comments | Reihe technischer Internet-Standards und Spezifikationen. |
| **SCAP** | Security Content Automation Protocol | Sammlung offener Standards für automatisierbares Schwachstellen- und Konfigurationsmanagement. |
| **XCCDF** | eXtensible Configuration Checklist Description Format | Standardformat für maschinenlesbare Sicherheitschecklisten und Konfigurationsprüfungen. |

## Zusätzlich verwendete Kürzel und Schreibweisen

| Kürzel | Bedeutung | Hinweis |
|---|---|---|
| **C2** | Command and Control | Infrastruktur, über die Angreifer kompromittierte Systeme fernsteuern. |
| **IT** | Informationstechnologie | Oberbegriff für technische Systeme zur Verarbeitung und Übertragung von Informationen. |
| **r / w / x** | read / write / execute | Typische Dateirechte, z. B. unter Linux: lesen, schreiben, ausführen. |
| **URI** | Uniform Resource Identifier | Standardisierte Zeichenfolge zur Identifikation einer Ressource; bei CPE als Namenssyntax genutzt. |
| **XML** | Extensible Markup Language | Auszeichnungssprache für strukturierte, maschinenlesbare Daten. |

## Verwechslungsgefahr

| Begriff | Nicht verwechseln mit |
|---|---|
| **CVE** | **CWE**: CVE benennt eine konkrete Schwachstelle; CWE beschreibt eine allgemeine Schwachstellenklasse. |
| **CVSS** | CVSS bewertet die technische Schwere; es ersetzt keine vollständige organisationsspezifische Risikobewertung. |
| **CERT / CSIRT** | Beide behandeln Sicherheitsvorfälle; CSIRT ist die allgemeinere Bezeichnung. |
| **IDS / IPS** | IDS meldet primär; IPS greift aktiv ein. |
| **DAC / RBAC / MAC** | DAC: Eigentümer entscheidet. RBAC: Rechte über Rollen. MAC: zentral/systembestimmt über Klassifizierung und Clearance. |
| **MAC** | Nicht mit der MAC-Adresse aus Netzwerken verwechseln; hier bedeutet MAC *Message Authentication Code*. |
