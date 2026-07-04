# IT-Sicherheit – Firewalls: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Firewalls“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **ACL** | Access Control List | Regelmenge für erlaubten bzw. verbotenen Netzwerkverkehr. |
| **CMD** | Command | SOCKS-Feld für gewünschte Operation, z. B. CONNECT oder BIND. |
| **DMZ** | Demilitarized Zone | Separates Netzsegment für öffentlich erreichbare Dienste. |
| **DNS** | Domain Name System | Namensauflösung von Domainnamen zu IP-Adressen. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll, oft bei WWW-Proxies gefiltert. |
| **HTTPS** | HTTP over TLS | Verschlüsseltes HTTP. |
| **IP** | Internet Protocol | Netzwerkprotokoll für Adressierung und Paketweiterleitung. |
| **IPv4** | Internet Protocol Version 4 | Ältere, weiterhin verbreitete IP-Version. |
| **IPv6** | Internet Protocol Version 6 | Neuere IP-Version mit größerem Adressraum. |
| **NAT** | Network Address Translation | Umsetzung zwischen internen und externen IP-Adressen. |
| **REP** | Reply | SOCKS-Serverantwort auf eine Verbindungsanfrage. |
| **SOCKS** | SOCKet Secure | Proxy-Protokoll auf Verbindungsebene. |
| **TCP** | Transmission Control Protocol | Verbindungsorientiertes Transportprotokoll. |
| **UDP** | User Datagram Protocol | Verbindungsloses Transportprotokoll. |
| **VER** | Version | SOCKS-Protokollversionsfeld. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Application-Level Proxy** | Proxy, der ein konkretes Anwendungsprotokoll versteht und prüfen kann, z. B. HTTP. |
| **Bastion Host** | Besonders gehärtetes, exponiertes System an einer Sicherheitsgrenze. |
| **Circuit-Level Proxy** | Proxy, der Verbindungen vermittelt, aber nicht zwingend Anwendungsdaten semantisch prüft. |
| **Default Deny** | Alles ist verboten, sofern keine explizite Regel es erlaubt. |
| **Defense in Depth** | Mehrere unterschiedliche Schutzschichten statt alleiniger Abhängigkeit von einer Firewall. |
| **Inbound Traffic** | Verkehr von außen in eine geschützte Zone. |
| **Lokale Firewall** | Firewall direkt auf einem Host oder Server. |
| **Outbound Traffic** | Verkehr aus einer geschützten Zone nach außen. |
| **Paketfilter** | Firewall, die Pakete anhand technischer Merkmale wie IP, Port und Protokoll filtert. |
| **Perimeterschutz** | Schutz an der Grenze zwischen internem Netz und externen Netzen. |
| **Port** | Logische Endpunktnummer eines Transportprotokolls, z. B. TCP-Port 80. |
| **Proxy** | Vermittler zwischen Client und Zielsystem. |
| **Screened Host** | Einfache Firewall-Architektur mit exponiertem Host hinter Paketfilter. |
| **Screened Subnet** | Architektur mit DMZ zwischen äußerer und innerer Firewall. |
| **Stateful Inspection** | Berücksichtigung des Zustands einer Verbindung, z. B. TCP-Session, bei Filterentscheidungen. |
| **WWW-Proxy** | Application-Level Proxy speziell für HTTP/HTTPS-Webzugriffe. |
| **Zwiebelschalenmodell** | Mehrschichtiges Schutzmodell mit mehreren Verteidigungslinien. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **Firewall / NAT** | Firewall kontrolliert Zugriffe; NAT übersetzt bzw. verbirgt Adressen. |
| **DMZ / Bastion Host** | DMZ ist ein Netzsegment; Bastion Host ist ein besonders gehärteter Host. |
| **Paketfilter / stateful Paketfilter** | Paketfilter prüft isolierte Pakete; stateful berücksichtigt Verbindungszustände. |
| **Paketfilter / Proxy** | Paketfilter kontrolliert Paketmerkmale; Proxy vermittelt komplette Kommunikation. |
| **Circuit-Level / Application-Level Proxy** | Circuit-Level prüft vor allem Verbindungsebene; Application-Level kennt Anwendungsprotokoll. |
| **Screened Host / Screened Subnet** | Screened Host ist einfach; Screened Subnet bietet DMZ und zwei Schutzgrenzen. |
| **Inbound / Outbound** | Inbound führt in das geschützte Netz; Outbound verlässt es. |
