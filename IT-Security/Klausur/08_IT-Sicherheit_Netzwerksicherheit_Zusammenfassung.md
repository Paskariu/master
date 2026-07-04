# IT-Sicherheit – Netzwerksicherheit: Schutzziele, Sniffing und Spoofing

> **Foliensatz:** `ITsecFR-1.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** Schutzziele im Netzwerk, Erkennung und Prävention, Sniffing, Spoofing, ARP-/Routing-/DNS-Spoofing sowie typische Analysewerkzeuge.

---

# 1. Überblick

Der Foliensatz führt in Netzwerksicherheit ein und behandelt:

- Schutzziele im Netzwerk
- Bedrohungen und ihre Erkennung
- Sniffing und Protokollanalyse
- Spoofing als Mittel zur Umleitung oder Täuschung
- Schutzmaßnahmen, insbesondere Firewall, Verschlüsselung, Segmentierung und Forensik

Leitfrage für jedes Schutzziel:

1. Was ist geschützt?
2. Wie wird es verletzt?
3. Wie lässt sich die Verletzung erkennen?
4. Wie lässt sie sich verhindern?

---

# 2. Schutzziele der Netzwerksicherheit

| Nr. | Schutzziel | Kernfrage |
|---|---|---|
| 1 | **Integrität** | Wurden Daten unautorisiert verändert? |
| 2 | **Vertraulichkeit** | Konnten Unbefugte Informationen oder Metadaten lesen? |
| 3 | **Verfügbarkeit** | Ist der Dienst für Berechtigte nutzbar? |
| 4 | **Verbindlichkeit / Nachweisbarkeit / Zurechenbarkeit** | Kann eine Handlung glaubhaft abgestritten werden? |
| 5 | **Authentizität** | Ist Subjekt oder Objekt wirklich die behauptete Identität? |
| 6 | **Privatheit / Anonymität / Pseudonymität** | Werden personenbezogene Daten und Identitätsbezüge angemessen geschützt? |

---

# 3. Integrität

## 3.1 Definition

Integrität bedeutet:

- Daten werden bei Übertragung nicht unautorisiert und unbemerkt verändert.
- Es existieren Regeln, wer welche Daten wie verändern darf.

## 3.2 Verletzung der Integrität

Mögliche Ursachen:

- Angreifer verändert Daten auf einem Kommunikationsweg.
- Man-in-the-Middle manipuliert Pakete.
- Technische Probleme, etwa schwache Signale oder Übertragungsfehler.

## 3.3 Erkennung

| Mittel | Aussage |
|---|---|
| **CRC** | Erkennt vor allem zufällige Übertragungsfehler; kein ausreichender Schutz gegen aktiven Angreifer. |
| **Hashwert** | Erkennt Veränderungen, wenn ein vertrauenswürdiger Originalhash vorhanden ist. |
| **Digitale Signatur** | Erkennt Manipulation und bindet Daten an den Signaturschlüsselinhaber. |

## 3.4 Verhinderung

- Unbefugten Netzzugang so weit wie möglich vermeiden.
- Übergänge zwischen Netzen absichern, z. B. mit Firewall.
- Kryptographische Integritätsmechanismen einsetzen: MACs, Signaturen, AEAD/TLS/IPsec.
- Grundsatz: Vollständiger Schutz durch reine Netzgrenzen ist nicht möglich; Angriffe können auch von innen kommen.

---

# 4. Vertraulichkeit

## 4.1 Definition

Vertraulichkeit bedeutet Schutz vor unautorisierter Informationsgewinnung.

Nicht nur Inhaltsdaten sind relevant, sondern auch **Metadaten**, z. B.:

- Wer kommuniziert mit wem?
- Wann?
- Wie lange?
- Über welche Systeme?
- Mit welchem Datenvolumen?

## 4.2 Verletzung

Typische Methode:

- Protokollieren und Mitschneiden von Datenverkehr (**Sniffing**).

## 4.3 Erkennung

Vertraulichkeitsverletzungen sind besonders schwierig zu erkennen:

- Passives Mitschneiden verändert Daten nicht.
- Oft gibt es keine sichtbaren Spuren.
- Schutz verlangt Kontrolle bzw. Vertrauen in die gesamte relevante Infrastruktur.

## 4.4 Verhinderung

- Datenverkehr über definierte und vertrauenswürdige Netzknoten führen.
- Verschlüsselung nutzen:
  - Anwendungsebene
  - TLS
  - IPsec
- Klartextprotokolle durch gesicherte Alternativen ersetzen.

---

# 5. Verfügbarkeit

## 5.1 Definition

Verfügbarkeit bedeutet, dass Funktionalität und Benutzbarkeit eines Dienstes erhalten bleiben.

## 5.2 Verletzung

| Ursache | Beispiel |
|---|---|
| Überlast | Zu viele legitime oder bösartige Anfragen. |
| Sabotage | Zerstörung oder Störung von Infrastruktur. |
| DoS | Gezielte Verfügbarkeitsbeeinträchtigung durch einzelne Quelle. |
| DDoS | Verteilte Überlastung durch viele Systeme, oft Botnetze. |

## 5.3 Erkennung

Verfügbarkeitsprobleme sind meist offensichtlich:

- Dienst reagiert nicht.
- Hohe Latenzen.
- Abgebrochene Verbindungen.
- Ressourcen sind ausgelastet.

## 5.4 Verhinderung

- Ausreichende Ressourcen und Redundanz.
- Rate Limiting / Begrenzung der Anfragefrequenz.
- Segmentierung:
  - intern vs. extern,
  - Abteilungen/Zonen,
  - kritische Dienste getrennt.
- QoS zur Priorisierung wichtiger Kommunikation.
- DDoS-Filterung und vorgelagerte Schutzdienste.

**Kernproblem:** Berechtigte und unberechtigte Zugriffe sind unter Last nicht immer eindeutig unterscheidbar.

---

# 6. Verbindlichkeit, Nachweisbarkeit und Zurechenbarkeit

## 6.1 Definition

Durchgeführte Handlungen sollen nicht glaubhaft abgestritten werden können.

Beispiele:

- „Ich habe diese E-Mail nicht gesendet.“
- „Ich habe diese Bestellung nicht ausgelöst.“
- „Mein Account wurde missbraucht.“

## 6.2 Ursachen und Angriffe

- Gestohlene Zugangsdaten.
- Malware, die im Namen des Nutzers handelt.
- Unzureichende Protokollierung.
- Gemeinsame Accounts.
- Fehlende oder schwache Authentisierung.

## 6.3 Erkennung

- Inkonsistenter Kontext:
  - ungewöhnlicher Ort,
  - ungewöhnliche Uhrzeit,
  - atypisches Gerät,
  - auffälliges Verhalten.
- Forensische Analyse von Logdaten und Beweisen.

## 6.4 Verhinderung

- Digitale Signaturen.
- Revisionssichere, manipulationsgeschützte Protokollierung.
- Beweissicherung und Forensik.
- Starke Authentisierung, getrennte individuelle Accounts.
- Angemessene rechtliche und organisatorische Prozesse.

**Wichtig:** Die erforderliche Stärke hängt vom rechtlichen und geschäftlichen Kontext ab.

---

# 7. Authentizität von Subjekten und Objekten

## 7.1 Definition

Authentizität bedeutet Echtheit der Identifikation von:

- **Subjekten**: z. B. Nutzer, Prozesse, Systeme.
- **Objekten**: z. B. Daten, Zertifikate, Hardware, Services.

Typische Nachweise:

- Passwort
- kryptographischer Schlüssel
- Biometrie
- Smartcard

## 7.2 Verletzung

- Passwort wird ausgespäht.
- Passwort wird gecrackt.
- Token/Smartcard wird gestohlen.
- Identität wird vorgetäuscht.
- Angreifer nutzt kompromittierte Credentials.

## 7.3 Erkennung

- Inkonsistenter Nutzungskontext.
- Ungewöhnliches Loginverhalten.
- Unbekannte Geräte, Standorte oder Zugriffsmuster.
- Mehrfache fehlgeschlagene Anmeldeversuche.

## 7.4 Verhinderung

- Stärkere Verfahren verwenden.
- Mehrere unabhängige Faktoren kombinieren (MFA).
- Karten/Token bei Verlust sperren.
- Schlüssel und Zugangsdaten austauschen.
- Geheimnisse sicher speichern.

---

# 8. Privatheit, Anonymität und Pseudonymität

## 8.1 Privatheit

Privatheit schützt personenbezogene Daten und das Recht auf informationelle Selbstbestimmung.

Risiken:

- unzureichend geschützte Zugriffe,
- Weiterverkauf von Daten für Werbung,
- umfassende Logfiles,
- Zweckänderung bei der Datenverarbeitung.

## 8.2 Anonymität

Anonymität bedeutet, dass die Identität einer Person nicht oder nicht zuverlässig feststellbar ist.

## 8.3 Pseudonymität

Pseudonymität bedeutet, dass eine Ersatzidentität genutzt wird. Eine Vertrauensstelle kann die Zuordnung zur realen Identität kennen.

Beispiel aus dem Foliensatz:

```text
Die reale Identität kennt nur eine vertrauenswürdige Stelle, etwa ein Notar.
```

## 8.4 Erkennung und Schutz

Erkennbar wird ein Privacy-Verstoß oft erst, wenn Daten in unerwartetem Kontext auftauchen.

Schutzmaßnahmen:

- Datenschutzgesetze.
- Zweckbindung.
- Datenminimierung.
- Schutz von Zugriffen.
- Pseudonyme.
- Reduktion/Schutz von Metadaten.

---

# 9. Sniffing

## 9.1 Definition

**Sniffing** bedeutet, Datenverkehr zu protokollieren und zu analysieren, ohne ihn zu verändern.

Es ist ein passiver Angriff auf die Vertraulichkeit.

## 9.2 Mögliche Abgriffspunkte

| Ort | Beispiel |
|---|---|
| Physische Übertragungsstrecke | Kabel, Glasfaser, Funk. |
| Layer 2 | Netzwerkkarte, Switch-Port, eigenes LAN. |
| Switch-Management | Port Mirroring/Management-Port. |
| Nach Spoofing | Angreifer leitet Verkehr über sich um. |
| Router/Switch auf Route | Protokollierung entlang der Internetroute. |
| Proxy | Zentraler Vermittler kann Inhalte sehen. |

## 9.3 Werkzeug: Wireshark

**Wireshark** ist ein Werkzeug zur Protokollanalyse.

Eigenschaften laut Folien:

- unterstützt sehr viele Protokolle,
- zeigt Protokollschichten transparent an,
- kann aufgezeichneten Netzwerkverkehr detailliert analysieren.

Klausurrelevanz:

| Klartext | Sicherere Alternative |
|---|---|
| HTTP | HTTPS/TLS |
| POP3 ohne TLS | POP3S oder STARTTLS |
| FTP | SFTP oder FTPS |
| Telnet | SSH |

**Merksatz:** Ein Passwort kann nur dann im Mitschnitt auftauchen, wenn es unverschlüsselt übertragen wird oder ein Endpoint kompromittiert ist.

---

# 10. Spoofing

## 10.1 Definition

**Spoofing** bedeutet, dass Angreifer Identitäts-, Adress- oder Zuordnungsinformationen fälschen.

Ziele:

- Verkehr umleiten,
- sich als ein anderes System ausgeben,
- Daten mitschneiden,
- Daten manipulieren,
- Verfügbarkeit beeinträchtigen.

## 10.2 Varianten

| Variante | Kerngedanke |
|---|---|
| **ARP Spoofing** | Falsche IP-zu-MAC-Zuordnungen im LAN. |
| **IP Spoofing** | Gefälschte Quell-IP-Adresse. |
| **Source Routing** | Manipulation bzw. Vorgabe des Übertragungswegs über IP-Optionen. |
| **Routing Table / BGP Spoofing** | Falsche Routeninformationen lenken Verkehr um. |
| **DNS Spoofing** | Gefälschte DNS-Antworten oder manipulierte DNS-Einstellungen. |

---

# 11. ARP Spoofing

## 11.1 Grundlage

ARP ordnet im lokalen Netzwerk einer IP-Adresse eine MAC-Adresse zu.

## 11.2 Angriffsidee

Der Angreifer sendet gefälschte ARP-Antworten:

```text
„Die IP des Gateways gehört zu meiner MAC-Adresse.“
„Die IP des Opfers gehört zu meiner MAC-Adresse.“
```

Dadurch speichern Opfer und Gateway falsche Zuordnungen.

## 11.3 Folge

Der Datenverkehr läuft über den Angreifer:

```text
Opfer <-> Angreifer <-> Gateway
```

Mögliche Folgen:

- Sniffing,
- Manipulation,
- Verwerfen von Paketen,
- Man-in-the-Middle im LAN.

## 11.4 Schutz

- Netzwerksegmentierung.
- Sicher konfigurierte Switches.
- Dynamic ARP Inspection, sofern verfügbar.
- Verschlüsselung, damit umgeleiteter Verkehr nicht lesbar/manipulierbar ist.
- Monitoring ungewöhnlicher ARP-Zuordnungen.

---

# 12. IP-, Routing- und BGP-Spoofing

## 12.1 IP Spoofing

Beim IP Spoofing wird die Quelladresse eines IP-Pakets gefälscht.

Anwendungen durch Angreifer:

- Verschleierung.
- Reflexions-/Amplification-Angriffe.
- DoS/DDoS.
- Umgehung schlechter, rein IP-basierter Vertrauensannahmen.

## 12.2 Source Routing

Source Routing ist eine IP-Option, mit der ein gewünschter Weg im Netz beeinflusst werden kann. Solche Optionen sind sicherheitskritisch und werden in vielen Umgebungen gefiltert oder deaktiviert.

## 12.3 Routing Table / BGP Spoofing

Bei Routing-Spoofing werden falsche Routeninformationen verbreitet.

Folge:

- Verkehr für ein Präfix wird über fremde Systeme geleitet.
- Angreifer können Verkehr beobachten, verändern oder nur stören.

**BGP-Hijacking** betrifft die globale Internet-Routingebene. Das Folienset betont: Es ist technisch detailreich, aber dauerhaft sicherheitsrelevant.

---

# 13. DNS Spoofing

## 13.1 Grundlage

DNS übersetzt Namen in IP-Adressen:

```text
www.company.com -> 172.16.32.64
```

## 13.2 Angriffsziel

Der Nutzer soll für einen korrekten Domainnamen eine falsche IP-Adresse erhalten:

```text
www.company.com -> Angreifer-IP
```

Danach kann der Angreifer Phishing, Man-in-the-Middle oder Datendiebstahl durchführen.

## 13.3 Ablauf des in den Folien gezeigten Angriffs

Die Grafiken auf Seiten 31–34 zeigen den Angriff als Race Condition:

1. Angreifer fragt zunächst seinen eigenen Hostnamen ab, um die aktuelle DNS-Query-ID zu bestimmen.
2. Angreifer löst eine Anfrage nach der Ziel-Domain aus und sendet sofort gefälschte DNS-Antworten.
3. Der Provider-Nameserver leitet die echte Anfrage an den zuständigen Nameserver weiter.
4. Die gefälschte Antwort trifft schneller ein als die echte Antwort; die echte Antwort wird verworfen.
5. Der Provider-Nameserver liefert die gefälschte IP an den Nutzer.
6. Nutzer wird auf das Angreifersystem umgeleitet.

## 13.4 Schutzideen

- DNSSEC zur kryptographischen Absicherung von DNS-Antworten.
- Sichere Resolver und aktuelle Randomisierung/Validierung.
- Monitoring auffälliger DNS-Antworten.
- Schutz und Kontrolle der Router-/DNS-Konfiguration.
- TLS mit korrekter Zertifikatsprüfung als zusätzliche Barriere gegen gefälschte Zielsysteme.

---

# 14. Zusammenhang der Maßnahmen

| Schutzziel | Typische Bedrohung | Erkennung | Schutz |
|---|---|---|---|
| Integrität | Manipulation, MITM | CRC, Hash, Signatur | Firewall, MAC/Signatur, Verschlüsselung |
| Vertraulichkeit | Sniffing | schwer bis nicht direkt erkennbar | TLS, IPsec, sichere Netzknoten |
| Verfügbarkeit | DoS/DDoS, Sabotage | Ausfall, Latenz, Monitoring | Ressourcen, Rate Limiting, Segmentierung, QoS |
| Nachweisbarkeit | Abstreiten, Malware | Kontext, Forensik | Signatur, Logs, Beweissicherung |
| Authentizität | Credential Theft, Spoofing | Kontextanomalien | MFA, Schlüssel, Smartcard, Sperrung |
| Privacy | Tracking, Datenabfluss | Daten in fremdem Kontext | Datenschutz, Zweckbindung, Pseudonyme |

---

# 15. Klausur-Checkliste

Du solltest zu diesem Foliensatz erklären können:

1. Alle sechs Schutzziele definieren.
2. Integrität, Vertraulichkeit und Verfügbarkeit mit Angriff, Erkennung und Gegenmaßnahme verbinden.
3. CRC, Hash und digitale Signatur hinsichtlich Integritätsprüfung abgrenzen.
4. Warum Vertraulichkeitsverletzungen durch Sniffing schwer erkennbar sind.
5. DoS und DDoS unterscheiden.
6. Warum Rate Limiting und Segmentierung die Verfügbarkeit schützen.
7. Verbindlichkeit, Nachweisbarkeit und Zurechenbarkeit einordnen.
8. Authentizität und typische Faktoren wie Passwort, Schlüssel, Biometrie und Smartcard erklären.
9. Privacy, Anonymität und Pseudonymität abgrenzen.
10. Sniffing definieren und mögliche Abgriffspunkte nennen.
11. Wireshark einordnen und Klartext-/verschlüsselte Protokolle unterscheiden.
12. Spoofing definieren und Varianten nennen.
13. ARP Spoofing als Man-in-the-Middle-Angriff erklären.
14. IP-/Routing-/BGP-Spoofing grob erklären.
15. DNS Spoofing als Race Condition in richtiger Reihenfolge erklären.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Netzwerksicherheit“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Themen: Schutzziele, Sniffing, Wireshark, Spoofing, ARP-, Routing- und DNS-Spoofing.
