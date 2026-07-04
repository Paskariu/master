# IT-Sicherheit – Netzwerksicherheit: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Netzwerksicherheit“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ARP** | Address Resolution Protocol | Ordnet im lokalen Netzwerk IP-Adressen zu MAC-Adressen zu. |
| **BGP** | Border Gateway Protocol | Routing-Protokoll zwischen autonomen Systemen im Internet. |
| **CRC** | Cyclic Redundancy Check | Prüfsumme zur Erkennung zufälliger Übertragungsfehler; kein kryptographischer Manipulationsschutz. |
| **DDoS** | Distributed Denial of Service | Verteilter Angriff auf die Verfügbarkeit über viele Systeme. |
| **DNS** | Domain Name System | Namensauflösung von Domainnamen zu IP-Adressen. |
| **DoS** | Denial of Service | Angriff zur Beeinträchtigung der Verfügbarkeit eines Dienstes. |
| **FTP** | File Transfer Protocol | Klassisches Dateiübertragungsprotokoll; ohne Zusatzschutz Klartext. |
| **FTPS** | FTP Secure | FTP mit TLS-Schutz. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll ohne eingebauten Kryptoschutz. |
| **HTTPS** | HTTP Secure | HTTP über TLS. |
| **IP** | Internet Protocol | Protokoll für Adressierung und Paketweiterleitung. |
| **IPsec** | Internet Protocol Security | Sicherheitsmechanismen auf Netzwerkschicht, häufig für VPNs. |
| **LAN** | Local Area Network | Lokales Netzwerk. |
| **MAC** | Media Access Control | Hardwareadresse auf Layer 2; relevant bei ARP. |
| **MITM** | Man-in-the-Middle | Angreifer sitzt zwischen Kommunikationspartnern und kann Verkehr mitlesen, verändern oder weiterleiten. |
| **MFA** | Multi-Factor Authentication | Kombination unabhängiger Authentifizierungsfaktoren. |
| **POP3** | Post Office Protocol Version 3 | E-Mail-Abrufprotokoll; ohne TLS Klartext. |
| **QoS** | Quality of Service | Priorisierung und Steuerung von Netzverkehr. |
| **SFTP** | SSH File Transfer Protocol | Dateiübertragung über SSH. |
| **SSH** | Secure Shell | Protokoll für sichere Remote-Verbindungen und Tunneling. |
| **TLS** | Transport Layer Security | Kryptographisches Protokoll für Vertraulichkeit, Integrität und Authentisierung von Verbindungen. |
| **VPN** | Virtual Private Network | Logisch geschütztes privates Netz über öffentliche Infrastruktur. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Anonymität** | Identität einer Person ist nicht oder nicht zuverlässig feststellbar. |
| **Authentizität** | Echtheit einer behaupteten Identität oder Herkunft. |
| **BGP Hijacking** | Falsche Routingankündigungen lenken Internetverkehr über fremde Systeme. |
| **CRC** | Fehlererkennung für zufällige Übertragungsfehler, aber nicht gegen aktive Angreifer. |
| **Data Integrity** | Schutz vor unautorisierten oder unbemerkten Datenänderungen. |
| **DNS Cache Poisoning** | Einschleusen gefälschter DNS-Zuordnungen in einen Resolver-Cache. |
| **DNS Spoofing** | Fälschen oder Manipulieren von DNS-Antworten, um Nutzer auf falsche IP umzuleiten. |
| **Firewall** | Kontrolliert Kommunikationsübergänge nach definierten Regeln. |
| **Forensik** | Sicherung, Analyse und Bewertung digitaler Spuren für Aufklärung und Beweisführung. |
| **Hashwert** | Kryptographischer Fingerabdruck von Daten. |
| **Integrität** | Daten werden nicht unautorisiert und unbemerkt verändert. |
| **Key/Schlüssel** | Geheimnis oder Teil eines kryptographischen Schlüsselpaares zur Sicherung von Kommunikation. |
| **Metadaten** | Kontextdaten über Kommunikation, z. B. Sender, Empfänger, Zeitpunkt und Datenvolumen. |
| **Nachweisbarkeit** | Möglichkeit, einen Vorgang und beteiligte Parteien später zu belegen. |
| **Non-Repudiation** | Nicht-Abstreitbarkeit: Handlung soll nicht glaubhaft abgestritten werden können. |
| **Pseudonymität** | Nutzung einer Ersatzidentität, die durch vertrauenswürdige Stelle ggf. aufgelöst werden kann. |
| **Privacy / Privatheit** | Schutz personenbezogener Daten und informationeller Selbstbestimmung. |
| **Race Condition** | Ergebnis hängt davon ab, welche konkurrierende Aktion bzw. Antwort zuerst eintritt. |
| **Rate Limiting** | Begrenzung der Anzahl von Anfragen pro Zeitspanne. |
| **Routing Table Spoofing** | Manipulation von Routinginformationen, damit Verkehr falsch weitergeleitet wird. |
| **Sniffing** | Passives Mitschneiden und Analysieren von Netzwerkverkehr. |
| **Source Routing** | Vorgabe/Manipulation des Wegs eines IP-Pakets über Routing-Informationen. |
| **Spoofing** | Fälschen von Identität, Herkunft, Adresse oder Zuordnung. |
| **Verbindlichkeit** | Handlungen bzw. Erklärungen können nicht glaubhaft abgestritten werden. |
| **Verfügbarkeit** | Systeme und Dienste sind für Berechtigte nutzbar. |
| **Vertraulichkeit** | Nur Berechtigte dürfen Informationen und relevante Metadaten kennen. |
| **Wireshark** | Netzwerk-Protokollanalysator zum Mitschneiden und Auswerten von Verkehr. |
| **Zurechenbarkeit / Accountability** | Handlungen können einem Subjekt zugeordnet werden. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Sniffing / Spoofing** | Sniffing liest passiv mit; Spoofing fälscht Identitäts- oder Zuordnungsinformationen. |
| **ARP Spoofing / IP Spoofing** | ARP Spoofing manipuliert IP-MAC-Zuordnungen im LAN; IP Spoofing fälscht die IP-Quelladresse. |
| **DNS Spoofing / BGP Hijacking** | DNS Spoofing manipuliert Namensauflösung; BGP Hijacking manipuliert Routingwege im Internet. |
| **DoS / DDoS** | DoS kann von einer Quelle kommen; DDoS verteilt die Überlast auf viele Systeme. |
| **Integrität / Authentizität** | Integrität: Daten wurden nicht verändert; Authentizität: Herkunft/Identität ist echt. |
| **Pseudonymität / Anonymität** | Bei Pseudonymität existiert grundsätzlich eine auflösbare Zuordnung; bei Anonymität nicht. |
| **CRC / Hash / Signatur** | CRC erkennt Zufallsfehler; Hash erkennt Manipulation bei vertrauenswürdigem Vergleichswert; Signatur ermöglicht zusätzlich Herkunftsnachweis. |
