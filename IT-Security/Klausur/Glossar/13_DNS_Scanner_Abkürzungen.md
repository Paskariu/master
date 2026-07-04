# IT-Sicherheit – DNS-Scanner: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„DNS-Scanner“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **C2 / C&C** | Command and Control | Infrastruktur, über die kompromittierte Systeme mit Angreifern kommunizieren. |
| **DFN** | Deutsches Forschungsnetz | Deutsches Wissenschaftsnetz; im Foliensatz im Zusammenhang mit DNS-RPZ erwähnt. |
| **DNS** | Domain Name System | System zur Auflösung von Domainnamen in IP-Adressen. |
| **DNS-RPZ** | DNS Response Policy Zone | Mechanismus zur Anwendung von DNS-basierten Antwort-/Filterrichtlinien. |
| **DoH** | DNS over HTTPS | DNS-Anfragen über HTTPS; kann zentrale DNS-Filter umgehen, wenn nicht kontrolliert. |
| **DoT** | DNS over TLS | DNS-Anfragen über TLS; ebenfalls für zentrale Filter relevant. |
| **EDR** | Endpoint Detection and Response | Endgeräteschutz zur Erkennung, Untersuchung und Reaktion auf Bedrohungen. |
| **HTTPS** | HTTP over TLS | Verschlüsseltes HTTP. |
| **IP** | Internet Protocol | Protokoll für Adressierung und Paketweiterleitung. |
| **TLS** | Transport Layer Security | Kryptographisches Protokoll für geschützte Verbindungen. |
| **URL** | Uniform Resource Locator | Webadresse einschließlich Protokoll, Domain und ggf. Pfad/Parameter. |
| **VPN** | Virtual Private Network | Geschützter Tunnel über öffentliche Infrastruktur; kann DNS-Filterung beeinflussen oder umgehen. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **Autoritativer Nameserver** | DNS-Server, der verbindliche Antworten für eine Domain/Zonen verwaltet. |
| **Blockliste / Negativliste** | Liste von Domains, die blockiert werden sollen. |
| **Blockseite** | Informationsseite, die bei einer Policy-Blockierung angezeigt wird. |
| **Command-and-Control Callback** | Verbindungsversuch von Malware zu Angreiferinfrastruktur. |
| **Content Filtering** | Filterung nach Inhalts- bzw. Nutzungskategorien, etwa Jugendschutz oder Unternehmenspolicy. |
| **Cryptomining** | Nutzung von Systemressourcen für Kryptowährungs-Mining. |
| **DNS-Resolver** | DNS-Komponente, die Anfragen rekursiv auflöst und Antworten an Clients liefert. |
| **DNS-Scanner / DNS-Sicherheitsfilter** | Resolver bzw. Dienst, der Domains vor der Auflösung gegen Sicherheits- und Inhaltsrichtlinien prüft. |
| **DNS Tunneling** | Missbrauch von DNS zur Datenübertragung oder Umgehung von Zugriffskontrollen. |
| **Domain** | Namensbereich wie `example.com`; DNS-Filter entscheiden typischerweise auf dieser Ebene. |
| **Domain-Reputation** | Bewertung einer Domain anhand von Bekanntheit, Verhalten, Kategorien und Bedrohungsdaten. |
| **Drive-by-Download** | Malware-Infektion durch Besuch einer präparierten oder kompromittierten Website. |
| **Dynamic DNS** | DNS-Dienst, der wechselnde IP-Adressen dynamisch einem Namen zuordnet. |
| **False Negative** | Schädliche Domain wird nicht erkannt bzw. nicht blockiert. |
| **False Positive** | Legitime Domain wird fälschlich blockiert. |
| **Kategorisierung** | Zuordnung von Domains zu Sicherheits- oder Inhaltskategorien. |
| **Lokaler Sicherheitsclient** | Agent auf dem Endgerät, der zentrale DNS-Policies ergänzen oder außerhalb des Unternehmensnetzes durchsetzen kann. |
| **Malware** | Schadsoftware oder Infrastruktur, die Schadsoftware/Exploits bereitstellt. |
| **Newly Seen Domain** | Kürzlich registrierte oder neu aktive Domain mit noch geringer Reputation. |
| **Phishing** | Täuschungsangriff zur Erlangung persönlicher, finanzieller oder Zugangsdaten. |
| **Positivliste / Allowlist** | Liste von explizit erlaubten Domains. |
| **Policy** | Regelwerk, nach dem Domains erlaubt, blockiert oder protokolliert werden. |
| **Root-Nameserver** | DNS-Server der Root-Zone, der auf zuständige Top-Level-Domain-Nameserver verweist. |
| **TLD** | Top-Level Domain | Oberste Domain-Endung, z. B. `.com`, `.de`. |
| **URL-Filter** | Filter, der Webadressen und ggf. Inhalte/Pfade prüfen kann. |
| **Webfilter** | Schutzmechanismus für Webzugriffe, häufig URL-/Inhaltsfilterung. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **DNS-Filter / URL-Filter** | DNS-Filter entscheidet meist nur über Domain; URL-Filter kann bis Pfad/Inhalt differenzieren. |
| **DNS-Scanner / DNSSEC** | DNS-Scanner bewertet Reputation/Policy; DNSSEC schützt DNS-Antworten gegen Manipulation. |
| **Domain / URL** | Domain ist z. B. `example.com`; URL umfasst zusätzlich Schema, Pfad und Parameter. |
| **Blockliste / Positivliste** | Blockliste verbietet bekannte Domains; Positivliste lässt nur explizit freigegebene Domains zu. |
| **Sicherheitsfilter / Inhaltsfilter** | Sicherheitsfilter blockiert z. B. Malware oder C2; Inhaltsfilter setzt Nutzungs-/Compliance-Regeln um. |
| **DNS Tunneling / VPN** | DNS Tunneling missbraucht DNS für Datenverkehr; VPN ist allgemein ein geschützter Netzwerk-Tunnel. |
| **Resolver / autoritativer Nameserver** | Resolver sucht Antworten für Clients; autoritativer Server liefert verbindliche Daten für seine Zone. |
