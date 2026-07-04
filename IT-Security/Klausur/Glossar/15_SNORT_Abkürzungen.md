# IT-Sicherheit – SNORT: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – SNORT“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **CVE** | Common Vulnerabilities and Exposures | Eindeutige Kennung einer konkreten bekannten Schwachstelle. |
| **DAQ** | Data Acquisition | Snort-Komponente/Schnittstelle zur Erfassung von Netzwerktraffic. |
| **DNS** | Domain Name System | Namensauflösung im Netzwerk; im Foliensatz für Cache-Poisoning-Regeln relevant. |
| **FIN** | Finish | TCP-Flag zum geordneten Verbindungsabbau. |
| **HIDS** | Host-based Intrusion Detection System | IDS, das Ereignisse auf einem einzelnen Host überwacht. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll; in Snort-Regeln für Header-/Webangriffe relevant. |
| **ICMP** | Internet Control Message Protocol | Kontrollprotokoll im IP-Netz; u. a. für OS-Fingerprinting relevant. |
| **IDS** | Intrusion Detection System | Erkennt und meldet verdächtige Aktivitäten. |
| **IPS** | Intrusion Prevention System | Erkennt und blockiert Angriffe aktiv. |
| **MITM** | Man-in-the-Middle | Angreifer sitzt zwischen Kommunikationspartnern und kann Kommunikation beeinflussen. |
| **NIDS** | Network-based Intrusion Detection System | IDS zur Überwachung von Netzwerkverkehr. |
| **NIPS** | Network-based Intrusion Prevention System | Netzwerk-IPS, das Verkehr inline blockieren kann. |
| **NXDOMAIN** | Non-Existent Domain | DNS-Antwortcode: angefragte Domain existiert nicht. |
| **SID / sid** | Signature ID | Eindeutige Kennung einer Snort-Regel. |
| **SYN** | Synchronize | TCP-Flag für Beginn eines Verbindungsaufbaus. |
| **TCP** | Transmission Control Protocol | Verbindungsorientiertes Transportprotokoll. |
| **TLS** | Transport Layer Security | Protokoll für verschlüsselte Netzwerkkommunikation. |
| **UDP** | User Datagram Protocol | Verbindungsloses Transportprotokoll. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Alert** | Von Snort erzeugte Meldung bei Treffer einer Regel oder Auffälligkeit. |
| **Anomalieerkennung** | Erkennung von Abweichungen von zuvor gelerntem oder erwartetem Normalverhalten. |
| **Classtype** | Regelattribut zur Einordnung eines Ereignisses, z. B. Reconnaissance oder attempted-user. |
| **Content Matching** | Suche nach Text- oder Bytefolgen im Netzwerkpayload. |
| **Detection Engine** | Snort-Komponente, die Pakete/Flows gegen Regeln prüft. |
| **Detection Filter** | Regeloption, die erst bei bestimmter Häufigkeit innerhalb eines Zeitfensters alarmiert. |
| **Directory Traversal** | Versuch, per Pfadmanipulation auf Dateien außerhalb des vorgesehenen Verzeichnisses zuzugreifen. |
| **False Negative** | Realer Angriff wird nicht erkannt. |
| **False Positive** | Legitime Aktivität wird fälschlich als Angriff gemeldet. |
| **Flow** | Regeloption für Kommunikationsrichtung und Verbindungszustand. |
| **Glastopf** | Webserver-Honeypot, der viele absichtlich angebotene Schwachstellen simuliert. |
| **Honeypot** | Kontrolliertes Lock-/Beobachtungssystem zur Erkennung und Analyse von Angriffen. |
| **Inline Mode** | Snort ist im Datenpfad und kann Pakete blockieren oder verwerfen. |
| **Metadata** | Zusatzinformationen in einer Regel, z. B. Dienst, Policy oder Regelquelle. |
| **NIDS Mode** | Snort prüft Traffic gegen Regeln und erzeugt Alerts. |
| **Option Node** | Interne Repräsentation konkreter Regelbedingungen unter einem gemeinsamen Rule Node. |
| **Packet Logger Mode** | Snort zeichnet Paketverkehr zur späteren Analyse auf. |
| **Passive Mode** | Sensor beobachtet Verkehr außerhalb des aktiven Datenpfads und blockiert nicht. |
| **Payload** | Nutzdaten eines Netzwerkpakets bzw. einer Anwendungskommunikation. |
| **Preprocessor** | Vorverarbeitung in Snort, z. B. Protokollanalyse, Normalisierung und Erkennung bestimmter Anomalien. |
| **Reconnaissance** | Aufklärungsphase eines Angriffs, z. B. Portscan, OS-Fingerprinting oder Dienstsuche. |
| **Reference** | Regelattribut mit Verweis auf externe Informationen wie CVE, Bugtraq oder URL. |
| **Revision / rev** | Versionsstand einer Snort-Regel. |
| **Rule Header** | Teil der Regel mit Action, Protokoll, Quelle, Ziel, Ports und Richtung. |
| **Rule Node** | Interne Gruppierung von Regeln mit gleichem Header. |
| **Rule Options** | Teil einer Snort-Regel mit Detailbedingungen und Meldung. |
| **Sensor** | System/Netzwerkpunkt, auf dem Snort Verkehr überwacht. |
| **Signature** | Regel bzw. Muster, das einen bekannten Angriff oder eine Auffälligkeit beschreibt. |
| **Sniffer Mode** | Snort zeigt oder analysiert erfasste Netzwerkpakete. |
| **Snorby** | GUI zur Analyse, Suche, Klassifikation und Administration von Snort-Events. |

## Wichtige Rule-Optionen

| Option | Bedeutung |
|---|---|
| `alert` | Erzeugt bei Regeltreffer eine Meldung. |
| `content` | Sucht ein bestimmtes Muster im Paket/Payload. |
| `depth` | Beschränkt die Content-Suche auf eine bestimmte Länge ab Beginn. |
| `offset` | Setzt die Startposition einer Content-Suche. |
| `flags` | Prüft TCP-Flags. |
| `flow` | Prüft Richtung und Zustand, z. B. `to_server`, `to_client`, `established`. |
| `msg` | Text der Alert-Meldung. |
| `byte_test` | Vergleicht Bytewerte an definierten Positionen. |
| `detection_filter` | Löst erst bei einer festgelegten Anzahl passender Ereignisse in einem Zeitraum aus. |
| `metadata` | Zusätzliche Einordnung der Regel. |
| `reference` | Externer Verweis, z. B. CVE. |
| `classtype` | Kategorie des Angriffsereignisses. |
| `sid` | Eindeutige Signatur-ID. |
| `rev` | Revision der Regel. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **IDS / IPS** | IDS meldet; IPS blockiert aktiv. |
| **NIDS / HIDS** | NIDS überwacht Netzwerktraffic; HIDS überwacht lokale Hostereignisse. |
| **Pattern / Anomalie** | Pattern erkennt bekannte Signaturen; Anomalie erkennt Abweichungen vom Normalverhalten. |
| **Passive / Inline Mode** | Passiv beobachtet; Inline kann Traffic beeinflussen/blockieren. |
| **Sniffer / Logger / NIDS Mode** | Sniffer betrachtet Traffic; Logger speichert ihn; NIDS prüft ihn gegen Regeln. |
| **Rule Header / Rule Options** | Header grenzt den relevanten Traffic ein; Options definieren Detailprüfung und Meldung. |
| **SID / Revision** | SID identifiziert Regel; Revision kennzeichnet Aktualisierungsstand. |
| **Alert / Incident** | Alert ist technischer Hinweis; Incident ist bestätigter Sicherheitsvorfall. |
| **False Positive / False Negative** | False Positive = Fehlalarm; False Negative = übersehener echter Angriff. |
