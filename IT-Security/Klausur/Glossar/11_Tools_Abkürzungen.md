# IT-Sicherheit – Sicherheitswerkzeuge: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Tools“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ACL** | Access Control List | Regelmenge zur Steuerung erlaubter/verbotener Kommunikation. |
| **CERT** | Computer Emergency Response Team | Team bzw. Quelle für Sicherheitswarnungen und Incident-Unterstützung. |
| **CVE** | Common Vulnerabilities and Exposures | Eindeutige Kennung für eine bekannte konkrete Schwachstelle. |
| **DMZ** | Demilitarized Zone | Getrennte Netzwerkzone für öffentlich erreichbare Dienste. |
| **GVM** | Greenbone Vulnerability Management | Suite/Plattform für Schwachstellenmanagement rund um Greenbone/OpenVAS. |
| **GPL** | GNU General Public License | Freie Softwarelizenz, unter der OpenVAS weiterentwickelt wurde. |
| **HIDS** | Host-based Intrusion Detection System | IDS, das einen einzelnen Host überwacht. |
| **IDS** | Intrusion Detection System | Erkennt und meldet Angriffe bzw. verdächtige Aktivitäten. |
| **IPS** | Intrusion Prevention System | Erkennt und blockiert Angriffe aktiv. |
| **IPsec** | Internet Protocol Security | Schutzmechanismen auf Netzwerkschicht, oft für VPNs. |
| **LAN** | Local Area Network | Lokales Netzwerk. |
| **NAT** | Network Address Translation | Übersetzung bzw. Verbergen interner IP-Adressen. |
| **NIDS** | Network-based Intrusion Detection System | IDS zur Überwachung von Netzwerkverkehr. |
| **Nmap** | Network Mapper | Werkzeug für Host-, Port- und Dienstscans. |
| **NSE** | Nmap Scripting Engine | Skriptumgebung von Nmap für zusätzliche Tests. |
| **PSK** | Pre-Shared Key | Vorab geteiltes Geheimnis; nicht zentral in diesem Satz, aber häufig bei Netzwerkzugriffen. |
| **SSH** | Secure Shell | Sicheres Remote-Login- und Tunneling-Protokoll. |
| **TLS** | Transport Layer Security | Kryptographisches Protokoll für geschützte Client-Server-Verbindungen. |
| **VLAN** | Virtual Local Area Network | Logische Trennung von Netzwerksegmenten. |
| **VPN** | Virtual Private Network | Geschütztes virtuelles Netz über öffentliche Infrastruktur. |

## Werkzeuge

| Werkzeug | Zweck |
|---|---|
| **Aircrack** | WLAN-Analyse und autorisierte Sicherheitstests. |
| **Cain & Abel** | Historisches Werkzeug für Sniffing, Passwort-Recovery und ARP-Spoofing. |
| **Greenbone** | Plattform für Schwachstellenmanagement; umfasst OpenVAS-nahe Komponenten. |
| **John** | Passwort-Hash-Cracking und Passworttests. |
| **Kali Linux** | Linux-Distribution mit zahlreichen Security-, Pentest- und Forensikwerkzeugen. |
| **Kismet** | WLAN-Erkennung und Analyse. |
| **Metasploit** | Framework für autorisierte Exploit-Tests und Sicherheitsforschung. |
| **Nessus** | Kommerzieller Schwachstellenscanner. |
| **Ophcrack** | Passwort-Recovery-Tool, historisch insbesondere für Windows-Hashes. |
| **OpenVAS** | Open Vulnerability Assessment System; Schwachstellen-/Konfigurationsscanner. |
| **rainbowcrack** | Tool für Rainbow-Table-basierte Passwort-/Hash-Angriffe. |
| **Scapy** | Werkzeug zur Erzeugung, Manipulation und Analyse von Netzwerkpaketen. |
| **Snort** | Signatur-/regelbasiertes NIDS/NIPS. |
| **Wireshark** | Paketmitschnitt und Protokollanalyse. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Broadcast Domain** | Bereich eines Netzes, in dem Broadcasts verteilt werden. |
| **Compliance Check** | Prüfung, ob Systeme vorgegebenen Sicherheits-/Konfigurationsanforderungen entsprechen. |
| **Computer Forensics** | Sicherung und Analyse digitaler Spuren zur Aufklärung von Vorfällen. |
| **Defense in Depth** | Kombination mehrerer unterschiedlicher Schutzschichten. |
| **False Negative** | Tatsächlicher Angriff wird nicht erkannt. |
| **False Positive** | Legitimer Vorgang wird fälschlich als Angriff erkannt. |
| **Host Scan** | Prüfung, ob ein System erreichbar bzw. aktiv ist. |
| **Malware Scan** | Suche nach bekannten oder verdächtigen Schadprogrammen bzw. Indikatoren. |
| **Penetration Testing** | Autorisierter Sicherheitscheck mit Angriffstechniken und definiertem Umfang. |
| **Port Scan** | Prüfung, welche Netzwerkports offen bzw. erreichbar sind. |
| **Reverse Engineering** | Analyse eines Programms oder Systems, um Aufbau/Funktion zu verstehen. |
| **Schwachstellenscan** | Automatisierte Suche nach bekannten Sicherheitslücken und Fehlkonfigurationen. |
| **Security Research** | Forschung und Analyse zu Sicherheitsmechanismen, Schwachstellen und Angriffen. |
| **Segmentierung** | Aufteilung von Netzen in getrennte Sicherheitszonen zur Begrenzung von Zugriffen und Ausbreitung. |
| **Signaturbasierte Erkennung** | Erkennung bekannter Angriffe anhand von Regeln oder Mustern. |
| **Switching** | Weiterleitung von Netzwerkverkehr über Switches; Grundlage für kontrollierte LAN-Strukturen. |
| **Virenscanner** | Software zur Erkennung, Quarantäne oder Entfernung von Malware. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Wireshark / Nmap** | Wireshark betrachtet vorhandenen Verkehr; Nmap sucht Systeme, Ports und Dienste. |
| **Nmap / OpenVAS** | Nmap ist primär Aufklärung/Portscan; OpenVAS ist umfassenderer Schwachstellenscanner. |
| **OpenVAS / Greenbone** | OpenVAS ist Scanner-/Projektbasis; Greenbone bezeichnet umfassendere Management-/Produktplattform. |
| **IDS / IPS** | IDS erkennt und meldet; IPS greift aktiv blockierend ein. |
| **Pentest / unerlaubter Angriff** | Pentest hat explizite Autorisierung und Scope; unerlaubte Tests sind Angriffe. |
| **Scan-Befund / bestätigte Verwundbarkeit** | Scannergebnis ist ein Hinweis; reale Ausnutzbarkeit muss geprüft werden. |
| **VLAN / DMZ** | VLAN ist logische Netztrennung; DMZ ist Sicherheitszone für exponierte Dienste. |
