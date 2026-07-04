# IT-Sicherheit 1 – Abkürzungen und Bedeutungen

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit 1 – Spezielle Bedrohungen“**.

| Abkürzung | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **AES** | Advanced Encryption Standard | Symmetrisches Verschlüsselungsverfahren; wird bei Ransomware typischerweise zur schnellen Verschlüsselung großer Datenmengen genutzt. |
| **ARP** | Address Resolution Protocol | Protokoll zur Zuordnung von IP- zu MAC-Adressen im lokalen Netzwerk. |
| **BGP** | Border Gateway Protocol | Routing-Protokoll zwischen autonomen Systemen im Internet; falsche Routenankündigungen können zu BGP-/IP-Hijacking führen. |
| **C2 / C&C** | Command and Control | Infrastruktur zur Fernsteuerung kompromittierter Systeme durch Angreifer. |
| **CMS** | Content Management System | System zur Verwaltung von Webinhalten; kann bei ungepatchten Schwachstellen ein Ransomware-Angriffsvektor sein. |
| **DDoS** | Distributed Denial of Service | Verteilter Angriff auf die Verfügbarkeit durch viele angreifende Systeme. |
| **DKIM** | DomainKeys Identified Mail | E-Mail-Authentisierungsverfahren mit kryptographischen Signaturen für Domains. |
| **DMARC** | Domain-based Message Authentication, Reporting and Conformance | Richtlinie für die Behandlung fehlgeschlagener SPF-/DKIM-Prüfungen und für Reporting. |
| **DNS** | Domain Name System | System zur Namensauflösung von Domains in IP-Adressen. |
| **DoS** | Denial of Service | Gezielte Störung oder Überlastung eines Dienstes bzw. Netzwerks. |
| **ECDH** | Elliptic Curve Diffie-Hellman | Asymmetrisches Verfahren zum Schlüsselaustausch auf Basis elliptischer Kurven. |
| **EDR** | Endpoint Detection and Response | Endpoint-Schutz zur Erkennung, Untersuchung und Reaktion auf verdächtige Aktivitäten. |
| **FTP** | File Transfer Protocol | Protokoll zur Dateiübertragung auf Anwendungsebene. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll auf Anwendungsebene. |
| **ICMP** | Internet Control Message Protocol | Internetprotokoll für Kontroll- und Fehlermeldungen; historisch bei Smurf Attack und Ping of Death relevant. |
| **IDS** | Intrusion Detection System | Erkennt Angriffe oder verdächtige Aktivitäten und meldet sie. |
| **IMAP** | Internet Message Access Protocol | Protokoll für den Zugriff auf E-Mails. |
| **IP** | Internet Protocol | Netzwerkprotokoll zur Adressierung und Weiterleitung von Paketen. |
| **LAN** | Local Area Network | Lokales Netzwerk, z. B. innerhalb eines Unternehmens oder Gebäudes. |
| **MAC** | Media Access Control | Hardwareadresse auf Layer 2; bei ARP wird eine IP-Adresse einer MAC-Adresse zugeordnet. |
| **MFA** | Multi-Factor Authentication | Authentisierung mit mehreren unabhängigen Faktoren, z. B. Passwort plus Token. |
| **MitB** | Man-in-the-Browser | Man-in-the-Middle-Angriff innerhalb eines kompromittierten Browsers. |
| **MITM** | Man-in-the-Middle | Angreifer sitzt zwischen Kommunikationspartnern, kann Daten mitlesen, verändern oder weiterleiten. |
| **mTAN** | mobile Transaction Authentication Number | Per Mobilgerät/SMS bereitgestellte TAN zur Transaktionsfreigabe. |
| **OS** | Operating System | Betriebssystem. |
| **OSI** | Open Systems Interconnection | Referenzmodell zur Einordnung von Netzwerkprotokollen in Schichten. |
| **PDF** | Portable Document Format | Dokumentformat; kann als präparierter Mail-Anhang Malware verbreiten. |
| **PIN** | Personal Identification Number | Persönliche Identifikationsnummer, z. B. für Banking oder Gerätezugang. |
| **POP** | Post Office Protocol | E-Mail-Abrufprotokoll. |
| **PRISM** | — | In der Folie als Beispiel auf der physischen Ebene genannt; kein allgemeines Netzwerkprotokoll. |
| **RFC** | Request for Comments | Reihe technischer Spezifikationen und Standards für Internet-Technologien. |
| **RSA** | Rivest-Shamir-Adleman | Asymmetrisches Kryptosystem; kann bei Ransomware zum Schutz symmetrischer Schlüssel verwendet werden. |
| **SMB** | Server Message Block | Netzwerkprotokoll für Datei- und Druckerfreigaben; EternalBlue/WannaCry nutzte eine SMB-Schwachstelle. |
| **SMTP** | Simple Mail Transfer Protocol | Protokoll zum Versand von E-Mails. |
| **SPAM** | — | Unerwünschte massenhaft versendete Nachrichten; keine offiziell einheitliche Langform. |
| **SPF** | Sender Policy Framework | DNS-basierte Prüfung, welche Server E-Mails für eine Domain versenden dürfen. |
| **SQL** | Structured Query Language | Sprache für relationale Datenbanken; SQL Injection manipuliert Datenbankabfragen. |
| **SYN** | Synchronize | TCP-Flag für den Beginn eines Verbindungsaufbaus; SYN Flood erzeugt viele halboffene Verbindungen. |
| **TAN** | Transaktionsnummer | Einmalcode zur Bestätigung einer Banking-Transaktion. |
| **TCP** | Transmission Control Protocol | Verbindungsorientiertes Transportprotokoll. |
| **UDP** | User Datagram Protocol | Verbindungsloses Transportprotokoll. |
| **USB** | Universal Serial Bus | Schnittstelle für Geräte und Speichermedien; möglicher Verbreitungsweg für Malware. |
| **WLAN** | Wireless Local Area Network | Drahtloses lokales Netzwerk. |
| **XSS** | Cross-Site Scripting | Einschleusen von Skripten in Webanwendungen, die im Browser anderer Nutzer ausgeführt werden. |

## Häufige Verwechslungsgefahr

| Begriff | Abgrenzung |
|---|---|
| **DoS / DDoS** | DoS kann von einem Angreifer ausgehen; DDoS verteilt die Last auf viele Systeme, häufig Botnetze. |
| **MITM / MitB** | MitB ist ein Spezialfall eines MITM-Angriffs: Die Manipulation findet im Browser des Opfers statt. |
| **ARP Cache Poisoning / IP-Spoofing** | ARP Poisoning manipuliert IP-MAC-Zuordnungen im lokalen Netz; IP-Spoofing fälscht die Quell-IP in Netzwerkpaketen. |
| **Phishing / Pharming** | Phishing lockt über gefälschte Nachrichten/Links; Pharming manipuliert Namensauflösung oder lokale Zuordnungen. |
| **Virus / Wurm** | Virus benötigt meist ein Wirtsprogramm bzw. Benutzeraktion; Wurm verbreitet sich selbstständig über Netzwerke. |
| **Trojaner / Backdoor** | Trojaner tarnt sich als nützliche Software; eine Backdoor ermöglicht unberechtigten Zugriff. |
| **RSA / AES** | RSA ist asymmetrisch und für Schlüsselaustausch/Schutz von Schlüsseln geeignet; AES ist symmetrisch und schnell für Nutzdaten. |
| **IDS / EDR** | IDS überwacht typischerweise Netzwerk oder Systeme auf Angriffe; EDR fokussiert Endgeräte und unterstützt Untersuchung/Reaktion. |
