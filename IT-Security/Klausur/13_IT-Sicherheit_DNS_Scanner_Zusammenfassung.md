# IT-Sicherheit – DNS-Scanner und DNS-basierte Filterung

> **Foliensatz:** `ITsecFR-FR DNS Scanner.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** DNS-basierte Sicherheitsfilter verstehen, von URL-Filtern abgrenzen, Funktionsweise, Schutzklassen und Grenzen erklären.

---

# 1. Ausgangsproblem

Nutzer verwenden viele Internetdienste. Dadurch steigt die Gefahr durch:

- Malware,
- Phishing,
- Command-and-Control-Kommunikation,
- Drive-by-Downloads,
- Kryptomining,
- schädliche oder unerwünschte Webseiten.

Eine Möglichkeit zum Schutz ist ein **URL-Filter**. Der Foliensatz nennt aber zwei zentrale Grenzen:

| Problem von URL-Filtern | Bedeutung |
|---|---|
| Schutz primär für Webzugriffe | Andere Dienste/Protokolle werden nicht automatisch abgedeckt. |
| Verschlüsselung | Bei HTTPS kann ein URL-Filter den konkreten Inhalt bzw. Pfad nicht ohne zusätzliche TLS-Inspection sehen. |

Alternative: Sicherheitskontrolle bereits bei der **DNS-Auflösung**.

---

# 2. Grundidee eines DNS-Scanners

Ein DNS-Scanner bzw. DNS-Sicherheitsfilter prüft Domain-Anfragen gegen Sicherheits- und Inhaltsrichtlinien.

Prinzip:

```text
Client
-> DNS-Anfrage für Domain
-> DNS-Sicherheitsresolver / DNS-Scanner
-> Policy-, Reputation- und Kategoriedaten
-> zulässige DNS-Antwort oder Blockierung
```

Der Client verwendet dazu einen alternativen bzw. zentral vorgegebenen DNS-Server.

Beispiele im Foliensatz:

- Cisco Umbrella,
- DNSSense,
- DFN / DNS-RPZ,
- vergleichbare DNS-Sicherheitsdienste.

---

# 3. DNS-Ablauf als Grundlage

Die Grafik auf Folie 3 zeigt den normalen DNS-Auflösungsweg:

1. Nutzer gibt eine Domain ein, z. B. `www.example.com`.
2. Der lokale DNS-Resolver erhält die Anfrage.
3. Resolver fragt Root-Nameserver nach zuständiger Top-Level-Domain.
4. Root verweist auf Nameserver der `.com`-TLD.
5. Resolver fragt den zuständigen autoritativen Nameserver.
6. Autoritativer Nameserver liefert die IP-Adresse.
7. Resolver gibt die IP-Adresse an Nutzer zurück.
8. Browser baut Verbindung zur Webadresse auf.
9. Webserver liefert die Webseite.

Ein DNS-Scanner sitzt typischerweise beim Resolver bzw. ersetzt diesen. Er kann die Auflösung daher blockieren, umleiten oder überwachen, bevor der Client die Ziel-IP erhält.

---

# 4. DNS-basierte Sicherheitsfilterung

## 4.1 Funktionsprinzip

1. Client fragt eine Domain beim konfigurierten DNS-Resolver an.
2. Resolver prüft die Domain gegen:
   - Positivlisten,
   - Negativlisten,
   - Reputationsdaten,
   - Kategorien,
   - Regeln bzw. KI-basierte Erkennung.
3. Ist die Domain erlaubt, wird die normale DNS-Antwort geliefert.
4. Ist sie blockiert, erhält der Client keine nutzbare Zielauflösung oder wird auf eine Block-/Informationsseite umgeleitet.

Wichtig: Die Filterentscheidung erfolgt grundsätzlich auf **Domain-Ebene**, nicht auf Ebene einzelner URL-Pfade.

## 4.2 Warum DNS-Filter auch bei HTTPS nützlich sind

Bei HTTPS ist der eigentliche Webseiteninhalt verschlüsselt. Ein DNS-Filter muss den Inhalt aber nicht entschlüsseln, weil er die Domain bereits vor Verbindungsaufbau prüfen kann.

Beispiel:

```text
Client fragt phishing.example an
-> Resolver erkennt Phishing-Domain
-> keine nutzbare Ziel-IP / Blockseite
-> HTTPS-Verbindung zur Zielseite entsteht gar nicht
```

Damit ist DNS-Filterung eine Alternative bzw. Ergänzung zu URL-Filterung ohne zwingende TLS-Inspection.

---

# 5. Sicherheitskategorien

Die Folie „Umbrella Security Settings“ zeigt typische Kategorien, die DNS-Sicherheitsdienste blockieren oder überwachen können.

| Kategorie | Bedeutung |
|---|---|
| **Malware** | Domains/Server, die Schadsoftware, Drive-by-Downloads, Exploits oder mobile Bedrohungen bereitstellen. |
| **Newly Seen Domains** | Sehr neue Domains; werden häufig in neuen Angriffskampagnen verwendet und haben noch geringe Reputation. |
| **Command and Control Callbacks** | Kommunikation kompromittierter Geräte mit Angreiferinfrastruktur wird verhindert. |
| **Phishing Attacks** | Betrügerische Websites, die Nutzer zur Preisgabe persönlicher oder finanzieller Daten verleiten. |
| **Dynamic DNS** | Domains mit dynamischer DNS-Infrastruktur; können legitim sein, werden aber oft auch missbräuchlich verwendet. |
| **Potentially Harmful Domains** | Domains mit verdächtigem Verhalten oder auffälliger Reputation. |
| **DNS Tunneling VPN** | DNS-basierte Tunnel bzw. VPN-Dienste, die Zugriffskontrollen oder Datenübertragungsregeln umgehen können. |
| **Cryptomining** | Zugriff auf Mining-Pools oder webbasierte Mining-Dienste wird kontrolliert bzw. blockiert. |

---

# 6. Inhalts- und Kategorienfilter

Zusätzlich zu sicherheitsbezogenen Kategorien können DNS-Dienste Inhaltskategorien blockieren.

Die Folie „Umbrella Content selection“ zeigt Beispiele wie:

- Adult / Pornography,
- Advertisements,
- Alcohol,
- Cannabis,
- Hate Speech,
- Illegal Activities,
- Illegal Downloads,
- Illegal Drugs,
- Gambling/Lotteries,
- Politics,
- Religion,
- Social/Professional Networking,
- Chat and Instant Messaging,
- Cloud and Data Centers,
- Internet of Things,
- Regional Restricted Sites,
- Safe for Kids.

## Zweck

Solche Filter können eingesetzt werden für:

- Jugendschutz,
- Unternehmensrichtlinien,
- Compliance,
- Begrenzung von Shadow IT,
- Blockierung rechtlich oder organisatorisch unerwünschter Inhalte.

## Konsequenz

DNS-Sicherheitsfilter verbinden technisch motivierte IT-Security mit inhaltlichen und organisatorischen Policies.

---

# 7. Blockierungsverhalten

Die Folie „Umbrella Meldung“ zeigt eine typische Blockseite.

Ablauf:

1. Nutzer versucht eine blockierte Domain aufzurufen.
2. DNS-Sicherheitsdienst erkennt die Kategorie, z. B. Phishing.
3. Zugriff wird blockiert bzw. auf eine Informationsseite umgeleitet.
4. Nutzer erhält eine Erklärung und gegebenenfalls Diagnoseinformationen.
5. Bei Fehlklassifizierung kann ein Kontakt zum IT-Administrator vorgesehen sein.

Vorteile:

- Nutzer erhält nicht nur einen technischen Fehler.
- Security Policy wird sichtbar gemacht.
- Fehlklassifizierungen können gemeldet werden.

---

# 8. Vorteile von DNS-Scannern

| Vorteil | Bedeutung |
|---|---|
| Einfache Einführung | DNS-Server beim Client, DHCP oder Netzwerk zentral konfigurieren. |
| Breiter Schutz | Nicht nur Browser, sondern viele DNS-nutzende Anwendungen profitieren. |
| Frühzeitige Blockierung | Verbindungsaufbau zur schädlichen Domain wird verhindert. |
| HTTPS-kompatibel | Schutz auf Domain-Ebene ohne Inhalte entschlüsseln zu müssen. |
| Zentrale Policy | Einheitliche Regeln für viele Nutzer und Geräte. |
| C2-Unterbindung | Malware kann Kommunikation mit Angreiferinfrastruktur verlieren. |
| Kategorisierung | Sicherheits-, Compliance- und Inhaltsrichtlinien zentral durchsetzbar. |
| Logging | DNS-Anfragen liefern Hinweise auf Malware, Phishing und Policy-Verstöße. |

---

# 9. Grenzen und Risiken

## 9.1 Nur Domain-Ebene

Ein DNS-Scanner kann laut Folien nicht zwischen verschiedenen Webseiten auf demselben Server bzw. derselben Domain unterscheiden.

Beispiel:

```text
example.org/harmlos
example.org/schaedlich
```

DNS sieht nur:

```text
example.org
```

Folge:

- Kein Pfad-/URL-Filtering.
- Blockierung kann zu grob sein.
- Eine Domain mit gemischten Inhalten ist schwer differenziert zu behandeln.

## 9.2 Policy-Abhängigkeit

Die Wirksamkeit hängt vom Betreiber und dessen Datenbasis ab:

- Welche Kategorien existieren?
- Wie schnell werden neue Bedrohungen erkannt?
- Wie hoch sind False Positives?
- Wie transparent ist die Klassifikation?
- Welche Domains werden geloggt und wie lange?

## 9.3 Umgehbarkeit

DNS-basierte Filter können umgangen oder geschwächt werden, wenn:

- Client andere externe DNS-Resolver nutzt,
- DNS-over-HTTPS/DNS-over-TLS nicht kontrolliert wird,
- Malware direkte IP-Adressen statt Domains verwendet,
- VPN/Tunnel oder eigene Resolver genutzt werden,
- kompromittierte Geräte lokale Einstellungen ändern.

## 9.4 Datenschutz

DNS-Anfragen zeigen oft:

- welche Dienste,
- welche Domains,
- wann,
- von welchem Nutzer bzw. Gerät

verwendet wurden.

Daher sind DNS-Logs datenschutzrelevant. Der Betreiber des Sicherheitsdienstes wird zu einer zentralen Vertrauensinstanz.

---

# 10. Ergänzung durch lokale Clients

Der Foliensatz nennt lokale Clients als Ergänzung.

Grundidee:

```text
zentraler DNS-Filter
+ lokaler Sicherheitsclient
= differenziertere Auswahl und zusätzliche Durchsetzung
```

Mögliche Aufgaben lokaler Clients:

- Policy auch außerhalb des Unternehmensnetzes anwenden,
- Nutzer/Gerät eindeutiger zuordnen,
- zusätzliche Kategorien/Regeln durchsetzen,
- Umgehungsversuche erkennen,
- Logging und Incident Response verbessern.

---

# 11. DNS-Scanner im Defense-in-Depth-Modell

Ein DNS-Scanner ersetzt keine anderen Sicherheitsmaßnahmen.

| Maßnahme | Ergänzung zum DNS-Scanner |
|---|---|
| Patchmanagement | verhindert Ausnutzung von Schwachstellen bei erreichtem Zielsystem. |
| Endpoint Protection / EDR | erkennt und blockiert Malware auf dem Endgerät. |
| Mail Security | reduziert Phishing-/Malware-Einstieg über E-Mail. |
| Firewall | kontrolliert Netzwerkverbindungen und Zonenübergänge. |
| TLS | schützt Vertraulichkeit/Integrität der Verbindung; DNS-Filter schützt vor schädlicher Domainauflösung. |
| URL-/Webfilter | kann URL-Pfad/Inhalt feiner prüfen, sofern technisch möglich. |
| Awareness | reduziert erfolgreiche Phishing- und Social-Engineering-Angriffe. |

**Merksatz:** DNS-Filter ist ein präventiver Kontrollpunkt am Anfang vieler Netzwerkverbindungen, aber kein vollständiger Malware- oder Webschutz.

---

# 12. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| URL-Filter / DNS-Filter | URL-Filter kann detaillierter bis URL-Pfad/Inhalt prüfen; DNS-Filter entscheidet primär auf Domain-Ebene. |
| DNS-Scanner / DNSSEC | DNS-Scanner bewertet Sicherheits-/Inhaltsreputation einer Domain; DNSSEC schützt Authentizität und Integrität von DNS-Antworten. |
| DNS-Scanner / Firewall | DNS-Scanner steuert Namensauflösung; Firewall steuert Netzwerkverkehr anhand von Regeln. |
| DNS-Filter / TLS-Inspection | DNS-Filter benötigt keine Inhaltsentschlüsselung; TLS-Inspection kann tiefere Webanalyse ermöglichen, ist aber komplexer und datenschutzsensibler. |
| Blockliste / Positivliste | Blockliste verbietet bekannte unerwünschte Domains; Positivliste erlaubt nur explizit freigegebene Domains. |
| Sicherheitskategorie / Inhaltskategorie | Sicherheitskategorie betrifft Malware/Phishing/C2; Inhaltskategorie betrifft Nutzungspolitik, Jugendschutz oder Compliance. |
| DNS-Tunneling / normales DNS | DNS-Tunneling nutzt DNS zur verdeckten Datenübertragung; normales DNS dient Namensauflösung. |

---

# 13. Klausur-Checkliste

Du solltest erklären können:

1. Warum URL-Filter bei HTTPS und bei nicht-webbasierten Diensten Grenzen haben.
2. Wie ein DNS-Scanner technisch eingebunden wird.
3. Den normalen DNS-Auflösungsweg grob erklären.
4. An welcher Stelle ein DNS-Sicherheitsresolver die Entscheidung trifft.
5. Warum DNS-Filter auch bei HTTPS funktionieren kann.
6. Malware, Newly Seen Domains, C2 Callbacks, Phishing, Dynamic DNS, DNS Tunneling und Cryptomining als Filterkategorien einordnen.
7. Sicherheits- und Inhaltsfilter unterscheiden.
8. Warum DNS-Filter nur auf Domain-Ebene arbeiten.
9. Vorteile und Grenzen von DNS-basiertem Schutz nennen.
10. Warum Policy-Qualität, Betreibervertrauen und Datenschutz wichtig sind.
11. Warum ein DNS-Scanner kein Ersatz für Firewall, Endpoint-Schutz, Patchmanagement oder Awareness ist.

---

## Quellenbasis

- Foliensatz **„DNS-Scanner“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Besonders relevant: DNS-Ablauf auf Folie 3, Sicherheitskategorien auf Folie 4, Inhaltskategorien auf Folie 5 und Grenzen auf Folie 7.
