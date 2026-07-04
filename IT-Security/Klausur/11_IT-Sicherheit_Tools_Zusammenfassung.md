# IT-Sicherheit – Sicherheitswerkzeuge und Maßnahmen

> **Foliensatz:** `ITsecFR-2c Tools.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** Sicherheitswerkzeuge korrekt einordnen, typische Einsatzzwecke kennen und technische sowie organisatorische Schutzmaßnahmen verbinden.

---

# 1. Überblick

Der Foliensatz behandelt Werkzeuge für:

- Mitschnitt und Analyse von Netzwerkverkehr,
- Port- und Dienstanalyse,
- Schwachstellenscans,
- Penetration Testing,
- Passwortangriffe,
- WLAN-Analyse,
- Intrusion Detection und Prevention,
- Forensik und Reverse Engineering.

Zusätzlich fasst er Sicherheitsmaßnahmen in drei Bereiche zusammen:

1. organisatorisch und strukturell,
2. Verschlüsselung und Authentifizierung,
3. Erkennung und Verfolgung.

---

# 2. Wireshark

## 2.1 Zweck

**Wireshark** ist ein Werkzeug zum Mitschneiden und Interpretieren von Netzwerkverkehr.

Typische Funktionen:

- Pakete aufzeichnen,
- Protokolle erkennen,
- Protokollschichten analysieren,
- Klartextdaten sichtbar machen,
- Fehler und ungewöhnlichen Traffic untersuchen.

## 2.2 Einsatz

Wireshark wird beispielsweise verwendet für:

- Fehlersuche in Netzwerken,
- Analyse von Verbindungen,
- Prüfung, ob Daten verschlüsselt übertragen werden,
- Untersuchung verdächtigen Verkehrs,
- Forensik und Incident Response.

## 2.3 Sicherheitsrelevanz

Wireshark selbst greift nicht zwingend an. Es kann aber zeigen, welche Informationen ein Angreifer bei unverschlüsselter Übertragung mitlesen könnte.

Beispiele:

| Unsicher bei Klartext | Bessere Alternative |
|---|---|
| HTTP | HTTPS / TLS |
| Telnet | SSH |
| FTP | SFTP oder FTPS |
| POP3 ohne TLS | POP3S / STARTTLS |

---

# 3. Nmap

## 3.1 Zweck

**Nmap – Network Mapper** dient zum Scannen von Hosts, offenen Ports, Diensten und teilweise bekannten Schwachstellen.

Es kann genutzt werden, um:

- aktive Systeme zu finden,
- offene Ports zu erkennen,
- Dienste und Versionen zu identifizieren,
- Netzwerkstrukturen zu erfassen,
- bestimmte Schwachstellen mit Skripten zu prüfen.

## 3.2 Zielangaben

Beispiele aus dem Foliensatz:

```text
127.0.0.1
```

Eigener Rechner / Loopback.

```text
192.168.1.0 255.255.255.0
```

Eigenes lokales Netz.

## 3.3 Profile und Skripte

Nmap bietet:

- vordefinierte Scan-Profile,
- eigene Profile,
- Skripte für konkrete Tests und Verwundbarkeiten,
- die **Nmap Scripting Engine (NSE)**.

Die Skriptsprache ist Lua.

## 3.4 Sicherheitsrelevanz

| Perspektive | Nutzen |
|---|---|
| Administrator / Defender | Eigene Angriffsfläche erkennen, offene Dienste reduzieren, Fehlkonfigurationen finden. |
| Angreifer | Aufklärung: erreichbare Hosts, Ports und Dienste identifizieren. |

**Merksatz:** Ein offener Port ist nicht automatisch eine Schwachstelle. Er zeigt zunächst nur, dass ein Dienst erreichbar ist.

---

# 4. Nessus, OpenVAS und Greenbone

## 4.1 Nessus

**Nessus** ist ein Schwachstellenscanner.

Laut Foliensatz:

- untersucht bekannte Schwachstellen,
- kann Compliance Checks durchführen,
- wurde 2005 unter proprietäre Lizenz gestellt.

## 4.2 OpenVAS

**OpenVAS – Open Vulnerability Assessment System** entstand als freie Weiterentwicklung.

Eigenschaften:

- nutzt bzw. integriert unter anderem Nmap,
- läuft auf Unix-Systemen,
- Bedienung meist über Webinterface,
- verwendet Feeds, unter anderem CERT-Informationen,
- liefert Hinweise auf CVEs.

## 4.3 Greenbone Vulnerability Management

OpenVAS ist seit 2017 Teil von **Greenbone Vulnerability Management (GVM)**.

Ziel:

- Systeme inventarisieren,
- Schwachstellen prüfen,
- Befunde priorisieren,
- Maßnahmen dokumentieren,
- wiederkehrende Scans durchführen.

## 4.4 Interpretation von Scan-Ergebnissen

Ein Scannerfund bedeutet nicht automatisch:

```text
„System ist sicher kompromittierbar.“
```

Der Fund ist zunächst ein Hinweis, der geprüft werden muss:

1. Ist die betroffene Software wirklich installiert?
2. Ist die genannte Funktion erreichbar?
3. Ist die Konfiguration tatsächlich verwundbar?
4. Gibt es kompensierende Kontrollen?
5. Wie hoch ist das Risiko im konkreten Systemkontext?

---

# 5. Kali Linux

## 5.1 Zweck

**Kali Linux** ist eine Linux-Distribution mit vorkonfigurierten Werkzeugen für:

- Penetration Testing,
- Security Research,
- Computer Forensics,
- Reverse Engineering.

Sie wird oft als virtuelle Maschine genutzt.

## 5.2 Enthaltene Tool-Kategorien

| Kategorie | Beispiele aus dem Foliensatz |
|---|---|
| Netzwerkanalyse | Wireshark, Nmap, Scapy |
| Passwortangriffe | John, Ophcrack, rainbowcrack |
| WLAN-Analyse | Aircrack, Kismet |
| Exploitation | Metasploit |
| IDS/IPS | Snort |
| Forensik / Reverse Engineering | zahlreiche spezialisierte Werkzeuge |

## 5.3 Einordnung

Kali Linux ist kein einzelnes Sicherheitswerkzeug, sondern eine Sammlung von Tools. Die Distribution ist für autorisierte Tests und Lernumgebungen gedacht.

---

# 6. Weitere Werkzeuge

## 6.1 Cain & Abel

Cain & Abel wird im Foliensatz genannt für:

- Sniffing,
- Passwort-Recovery, z. B. aus Caches,
- ARP-Spoofing.

Klausurrelevanz:

- verbindet Passwortangriffe und Netzwerkangriffe,
- kann bei ARP-Spoofing eine Man-in-the-Middle-Position im LAN ermöglichen.

## 6.2 Passwortangriffswerkzeuge

| Werkzeug | Einordnung |
|---|---|
| **John** | Passwort-Hash-Cracking, z. B. Wörterbuch- und Brute-Force-Angriffe. |
| **Ophcrack** | Passwort-Recovery, insbesondere historisch für Windows-Hashes. |
| **rainbowcrack** | Nutzung von Rainbow Tables für vorberechnete Passwort-/Hash-Angriffe. |

## 6.3 WLAN-Werkzeuge

| Werkzeug | Einordnung |
|---|---|
| **Aircrack** | Analyse und Angriffstests gegen WLAN-Sicherheitsmechanismen. |
| **Kismet** | Drahtlose Netzwerkerkennung und WLAN-Analyse. |

## 6.4 Exploitation und Paketmanipulation

| Werkzeug | Einordnung |
|---|---|
| **Metasploit** | Framework zum Entwickeln, Testen und Ausführen bekannter Exploits in autorisierten Umgebungen. |
| **Scapy** | Python-basiertes Werkzeug zur Erstellung, Manipulation und Analyse von Netzwerkpaketen. |

---

# 7. Snort: IDS und IPS

## 7.1 Begriffe

| System | Aufgabe |
|---|---|
| **IDS – Intrusion Detection System** | Erkennt verdächtige Aktivitäten und erzeugt Meldungen. |
| **IPS – Intrusion Prevention System** | Erkennt Angriffe und blockiert bzw. verhindert sie aktiv. |
| **NIDS – Network IDS** | Überwacht Netzwerkverkehr. |
| **HIDS – Host IDS** | Überwacht einen einzelnen Host, z. B. Logs, Dateien und Prozesse. |

## 7.2 Snort

**Snort** ist ein Werkzeug, das als NIDS und – je nach Betriebsmodus – als NIPS eingesetzt werden kann.

Typische Eigenschaften:

- Signatur-/regelbasierte Erkennung,
- Analyse von Paketen und Protokollen,
- Alarmierung bei verdächtigen Mustern,
- optional aktive Blockierung im Inline-Betrieb.

## 7.3 Grenzen von IDS/IPS

| Problem | Bedeutung |
|---|---|
| False Positives | Legitimer Verkehr wird fälschlich als Angriff bewertet. |
| False Negatives | Tatsächlicher Angriff wird nicht erkannt. |
| Neue Angriffsmuster | Signaturbasierte Systeme erkennen unbekannte Varianten schlechter. |
| Verschlüsselter Traffic | Tiefere Inhaltsanalyse kann ohne TLS-Inspection eingeschränkt sein. |

---

# 8. Sicherheitsmaßnahmen: Struktur und Organisation

Der Foliensatz ordnet Maßnahmen in drei Kategorien.

## 8.1 Organisatorisch und strukturell

| Maßnahme | Zweck |
|---|---|
| Segmentierung | Trennung von Sicherheitszonen und Begrenzung lateraler Bewegung. |
| Switching | Kontrollierter Verkehr im LAN; reduziert ungezielte Broadcast-/Abhörmöglichkeiten gegenüber Hubs. |
| Broadcast-Domänen | Begrenzen die Reichweite von Broadcast-Verkehr. |
| VLANs | Logische Trennung von Netzen auf gemeinsamer physischer Infrastruktur. |
| NAT | Verbirgt interne Adressstrukturen, ersetzt aber keine Firewall. |
| DMZ | Separate Zone für öffentlich erreichbare Dienste. |
| Bastion Host | Besonders gehärteter, exponierter Host. |
| Firewall | Durchsetzung von Kommunikationsregeln am Übergang zwischen Zonen. |
| Lokale Firewall | Zusätzlicher Schutz direkt auf Endgeräten/Servern. |
| Virenscanner | Erkennung und ggf. Blockierung bekannter Malware in Dateien/Webzugriffen. |

## 8.2 Verschlüsselung und Authentifizierung

| Maßnahme | Einsatz |
|---|---|
| Leitungsverschlüsselung | Schutz fester Verbindungen zwischen Endpunkten. |
| IPsec | Schutz auf IP-/Netzwerkschicht, oft VPN. |
| TLS | Geschützte Client-Server-Kommunikation, z. B. HTTPS. |
| SSH | Sicherer Remote-Zugriff und Tunneling auf Anwendungsebene. |

## 8.3 Erkennung und Verfolgung

| Maßnahme | Zweck |
|---|---|
| Tools | Analyse, Scans, Forensik und Prüfung. |
| IDS/IPS | Angriffe erkennen bzw. blockieren. |
| Sicherheitsforen/Listen | Informationen über Schwachstellen, Angriffe und Gegenmaßnahmen beobachten. |
| Logging und Monitoring | Nachvollziehbarkeit, Alarmierung und Incident Response. |

---

# 9. Zusammenhänge

```text
Nmap / OpenVAS / Greenbone
-> Schwachstellen und Angriffsfläche erkennen

Wireshark / Scapy
-> Netzwerkverkehr analysieren oder gezielt untersuchen

Snort
-> Angriffe im Betrieb erkennen oder blockieren

Kali Linux
-> bündelt viele Sicherheitswerkzeuge für autorisierte Tests

Firewalls / VLANs / DMZ / Host-Firewalls
-> Angriffswege und Ausbreitung begrenzen

TLS / IPsec / SSH
-> Kommunikation schützen
```

**Merksatz:** Sicherheit entsteht nicht durch ein einzelnes Tool. Sie erfordert präventive, detektive und reaktive Maßnahmen als Defense in Depth.

---

# 10. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| Wireshark / Nmap | Wireshark analysiert beobachteten Verkehr; Nmap sucht aktive Hosts, Ports und Dienste. |
| Nmap / OpenVAS | Nmap fokussiert Aufklärung und Port-/Dienstanalyse; OpenVAS bewertet viele bekannte Schwachstellen und Konfigurationen. |
| OpenVAS / Greenbone | OpenVAS ist Scanner-/Projektbasis; GVM ist die umfassendere Greenbone-Suite bzw. Plattform. |
| IDS / IPS | IDS meldet; IPS blockiert aktiv. |
| Snort / Wireshark | Snort erkennt Muster im laufenden Verkehr; Wireshark analysiert Mitschnitte detailliert. |
| Kali Linux / einzelnes Tool | Kali ist eine Distribution mit vielen Tools, kein einzelnes Tool. |
| Pentest / Angriff | Ein Pentest erfolgt mit Autorisierung und definiertem Umfang; ein Angriff ohne Erlaubnis ist unzulässig. |
| Scan-Fund / bestätigte Schwachstelle | Ein Scan-Fund ist ein Hinweis; Ausnutzbarkeit und Risiko müssen validiert werden. |

---

# 11. Klausur-Checkliste

Du solltest erklären können:

1. Wireshark, Nmap, Nessus/OpenVAS/Greenbone, Kali Linux und Snort zuordnen.
2. Unterschied zwischen Netzwerkanalyse, Portscan und Schwachstellenscan.
3. Warum Scannergebnisse validiert werden müssen.
4. Wofür die Nmap Scripting Engine genutzt wird.
5. Kali Linux als Werkzeug-Sammlung einordnen.
6. Cain & Abel, John, Ophcrack, rainbowcrack, Aircrack, Kismet, Metasploit und Scapy grob zuordnen.
7. IDS und IPS abgrenzen.
8. Warum Snort signaturbasiert nicht jeden unbekannten Angriff erkennt.
9. VLAN, DMZ, Bastion Host, NAT und Firewall als Strukturmaßnahmen einordnen.
10. Warum Defense in Depth statt eines einzelnen Tools nötig ist.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Tools“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Themen: Wireshark, Nmap, Nessus/OpenVAS/Greenbone, Kali Linux, Snort sowie organisatorische, kryptographische und detektive Sicherheitsmaßnahmen.
