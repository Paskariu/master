# IT-Sicherheit 1 – Spezielle Bedrohungen

> **Foliensatz:** `1-spezielle_bedrohungen_v6.pdf`  
> **Inhalt:** Systematik von Bedrohungen, konkrete Netzwerkangriffe, Schadsoftware und Human Factors.  
> **Klausurfokus:** Begriffe sauber abgrenzen, Angriffstypen zuordnen, Ablauf und Gegenmaßnahmen erklären.

---

## 1. Systematik von Bedrohungen und Angriffen
> Systematik aus **RFC 4949**. Bedrohungen werden nach ihrer **Folge** (*threat consequence*) unterschieden.

| Hauptklasse | Kerngedanke | Hauptsächlich gefährdete Schutzziele |
|---|---|---|
| **Unauthorized Disclosure** | Unautorisierte Informationsgewinnung | Vertraulichkeit, Privacy |
| **Deception** | Täuschung von Personen oder Systemen | Authentizität, Integrität, Nachweisbarkeit |
| **Disruption** | Unterbrechung, Störung oder Funktionsbeeinträchtigung | Verfügbarkeit, Integrität |
| **Usurpation** | Widerrechtliche Aneignung oder missbräuchliche Nutzung | Vertraulichkeit, Integrität, Verfügbarkeit, Autorisierung |

---

## 1.2. Unauthorized Disclosure – unautorisierte Informationsgewinnung
> Ziel: Informationen ohne Berechtigung erhalten.

| Untertyp | Bedeutung | Beispiel |
|---|---|---|
| **Exposure** | Sensitive Informationen werden beabsichtigt oder unbeabsichtigt offengelegt. | Fehlversand, öffentlich erreichbare Datei, Leak. |
| **Interception** | Datenübertragung oder Datenträger werden abgefangen. | Sniffing, Abhören, Diebstahl eines Laptops/USB-Sticks. |
| **Inference** | Indirekte Informationsgewinnung durch logisches Schließen. | Verkehrsflussanalyse: Wer kommuniziert wann mit wem und wie viel? |
| **Intrusion** | Schutzmechanismen werden umgangen, um Informationen oder Zugriff zu erhalten. | Physisches Eindringen, Reverse Engineering. |

### Wichtige Abgrenzung
- **Interception**: Daten werden direkt mitgelesen oder abgefangen.
- **Inference**: Informationen werden aus beobachtbaren Nebeninformationen abgeleitet.
- **Exposure**: Information wird sichtbar, ohne dass zwingend ein aktiver Angriff auf ein System nötig ist.
- **Intrusion**: Schutzbarriere wird aktiv überwunden.

---

## 1.3. Deception – Täuschung
>Ziel: Ein System oder eine Person wird dazu gebracht, einer falschen Identität, einer falschen Information oder einem falschen Vorgang zu vertrauen.

| Untertyp | Bedeutung | Beispiel |
|---|---|---|
| **Masquerade / Maskierung** | Angreifer gibt sich als andere Identität oder legitimer Dienst aus. | Spoofing, Trojanisches Pferd, gefälschte Mail-Absenderadresse. |
| **Falsification** | Daten werden verändert oder eingefügt. | Manipulation von Buchungsdaten, Änderung einer Kontonummer. |
| **Repudiation** | Durchführung einer Aktion wird abgestritten. | „Ich habe diese Bestellung/Mail nicht gesendet.“ |

### Begriff: Spoofing
> **Spoofing** bedeutet das Vortäuschen einer falschen Identität oder Herkunft.

Beispiele:
- E-Mail-Spoofing
- IP-Spoofing
- DNS-Spoofing
- Website-Spoofing

Spoofing verletzt primär **Authentizität**. Es kann aber auch die Grundlage für weitere Verletzungen von Vertraulichkeit oder Integrität sein.

---

## 1.4. Disruption – Unterbrechung und Störung
> Ziel: Verfügbarkeit oder korrekte Funktionsweise beeinträchtigen.

| Untertyp           | Bedeutung                                                                  | Ursachen / Beispiele                                                                        |
| ------------------ | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Incapacitation** | Systemkomponente wird untauglich gemacht.                                  | Schadsoftware, physische Zerstörung, Bedienfehler, Hardware-/Softwarefehler, höhere Gewalt. |
| **Corruption**     | Funktionsweise wird durch Veränderung von Software oder Daten beeinflusst. | Tampering, Schadsoftware, Fehler, Naturereignisse.                                          |
| **Obstruction**    | System oder Kommunikation wird behindert bzw. überlastet.                  | DoS, DDoS, Netzstörung.                                                                     |

### Wichtige Begriffe
- **Malicious Logic**: Schadlogik bzw. bösartige Logik in Software.
- **Tampering**: gezielte Manipulation eines Systems, Programms oder von Daten.
- **DoS**: gezielte Verfügbarkeitsbeeinträchtigung durch Überlastung oder Störung.
- **DDoS**: verteilter DoS durch viele angreifende Systeme.

---

## 1.5. Usurpation – widerrechtliche Aneignung
> Ziel: Systeme oder Ressourcen unbefugt für eigene Zwecke verwenden.

| Untertyp             | Bedeutung                                                                   | Beispiel                                                                                               |
| -------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Misappropriation** | Unautorisierte Nutzung oder „Unterschlagung“ von Ressourcen.                | Rechenleistung für Krypto-Mining, Datendiebstahl.                                                      |
| **Misuse**           | Missbrauch einer Komponente oder Überschreitung der eigenen Berechtigungen. | Admin verwendet Berechtigungen für private/illegitime Zwecke; Nutzer greift außerhalb seiner Rolle zu. |

---

# 2. Bedrohungen nach Schichten

Die Folie ordnet Beispiele der OSI- und TCP/IP-Schichten zu.

| Ebene                        | Protokollbeispiele                   | Bedrohungsbeispiele                                                                                  |
| ---------------------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| **„Layer 8: User“**          | —                                    | Phishing, Social Engineering, Scareware, Hoax, Identitätsdiebstahl, Spam                             |
| **Application Layer**        | HTTP, FTP, IMAP, SMTP, POP, DNS, BGP | E-Mail-Spoofing, Session Hijacking, DNS-Amplification, BGP/IP-Hijacking, SQL Injection, XSS, Malware |
| **Transport Layer**          | TCP, UDP                             | SYN Flood, DDoS                                                                                      |
| **Network / Internet Layer** | IP, ICMP                             | IP-Spoofing, Smurf Attack, IP Fragmentation Attack, Ping of Death                                    |
| **Data Link Layer**          | Ethernet, WLAN                       | ARP Cache Poisoning                                                                                  |
| **Physical Layer**           | Kabel, Glasfaser, Funk               | Abhören / physischer Zugriff, z. B. PRISM-Kontext                                                    |

## 2.1 Layer 8: Der Mensch

Viele Sicherheitsvorfälle funktionieren nicht primär durch technische Schwachstellen, sondern durch die Manipulation von Menschen:

- Phishing
- Social Engineering
- Scareware
- Hoaxes
- Identitätsdiebstahl
- Spam

**Merksatz:** Der Nutzer ist kein OSI-Layer, aber praktisch oft der wirksamste Angriffsweg.

## 2.2 Application Layer

| Angriff | Kurzbeschreibung |
|---|---|
| **Session Hijacking** | Übernahme einer bestehenden Sitzung, z. B. durch gestohlene Session-ID/Cookie. |
| **DNS Amplification Attack** | Reflexions-/Verstärkungsangriff: kleine Anfrage erzeugt große Antwort an das Opfer. |
| **BGP/IP Hijacking** | Falsche Routingankündigungen lenken Internetverkehr um. |
| **SQL Injection** | Manipulation von Datenbankabfragen über unsichere Eingaben. |
| **XSS – Cross-Site Scripting** | Einschleusen von Skripten in Webseiten, die im Browser anderer Nutzer laufen. |

## 2.3 Transport Layer: SYN Flood

Bei TCP wird eine Verbindung gewöhnlich über den Three-Way Handshake aufgebaut:

```text
Client -> Server: SYN
Server -> Client: SYN-ACK
Client -> Server: ACK
```

Beim **SYN Flood** sendet der Angreifer viele SYN-Pakete, beendet den Handshake aber nicht. Der Server hält Ressourcen für halboffene Verbindungen vor; dadurch können echte Nutzer keinen Dienst mehr erhalten.

**Schutzziel:** Verfügbarkeit.

## 2.4 Network Layer

| Angriff | Idee |
|---|---|
| **IP Address Spoofing** | Manipulierte Quell-IP-Adresse. |
| **Smurf Attack** | ICMP-Echo-Anfrage an Broadcast-Adresse mit gefälschter Quell-IP des Opfers; viele Antworten überlasten das Opfer. |
| **IP Fragmentation Attack** | Missbrauch von Fragmentierung, um Filter/Firewalls zu umgehen oder fehlerhafte Verarbeitung auszulösen. |
| **Ping of Death** | Historischer Angriff durch übergroße bzw. fehlerhaft zusammengesetzte ICMP-Pakete. |

## 2.5 Data Link Layer: ARP Cache Poisoning
> ARP ordnet im lokalen Netzwerk IP-Adressen MAC-Adressen zu.

**ARP Cache Poisoning**:
1. Angreifer sendet gefälschte ARP-Antworten.
2. Opfer speichert falsche IP-MAC-Zuordnung.
3. Verkehr wird an Angreifer statt an Gateway oder Zielsystem gesendet.
4. Angreifer kann Verkehr mitschneiden, verändern oder verwerfen.

Folge: häufig **Man-in-the-Middle** im LAN.

---

# 3. Schadsoftware / Malware

## 3.1 Definition

**Malware** ist Schadsoftware, die Systeme kompromittiert, Informationen ausspäht, Funktionen stört, Zugriff ermöglicht oder sich verbreitet.

## 3.2 Verbreitungswege
- E-Mail-Anhänge, trotz Spam-Filter
- Präparierte Office- oder PDF-Dokumente
- Archive, Skripte oder ausführbare Dateien
- **Drive-by-Download** über kompromittierte Webseiten
- Ausnutzung von Schwachstellen in Betriebssystem, Browser oder Plug-ins
- USB-Stick
- LAN / Netzwerkfreigaben
- Überspringen auf mobile Geräte
- Nachladen weiterer Schadsoftware durch bereits vorhandene Malware/Bot

### Drive-by-Download
Der Nutzer landet auf einer kompromittierten Website – direkt oder über Umleitung, iframe oder bösartige Werbung. Ein Exploit Kit nutzt dann Sicherheitslücken im Browser, Plug-in oder Betriebssystem aus.

Der Nutzer muss die Schadsoftware nicht bewusst starten.

### Warum Signaturerkennung nicht ausreicht
Malware wird häufig individualisiert oder leicht verändert. Dadurch ändern sich Dateihash, Struktur oder Signaturen, während die Schadwirkung erhalten bleibt. Rein signaturbasierte Scanner erkennen solche Varianten nicht zuverlässig.

---

## 3.3 Malware-Typen

| Typ                                 | Definition                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Virus**                           | Verbreitet sich durch Anhängen an ein Wirtsprogramm. Benötigt typischerweise dessen Ausführung.         |
| **Wurm**                            | Verbreitet sich selbstständig über Netzwerke.                                                           |
| **Trojanisches Pferd**              | Bietet vordergründig nützliche Funktionalität, enthält aber versteckte Zusatzfunktionen, z. B. Spyware. |
| **Logische Bombe**                  | Schadfunktion wird beim Eintritt einer bestimmten Bedingung aktiviert.                                  |
| **Zeitbombe**                       | Sonderfall der logischen Bombe: Aktivierung zu einem bestimmten Zeitpunkt.                              |
| **Hintertür / Trapdoor / Backdoor** | Ermöglicht unberechtigten Zugang zu Funktionen oder Systemen.                                           |
| **Spyware**                         | Späht Informationen aus, oft versteckt als Teil eines Trojaners.                                        |
| **Dropper**                         | Erstes Schadprogramm, das den eigentlichen Payload nachlädt.                                            |

### Virus vs. Wurm

| Virus | Wurm |
|---|---|
| Benötigt Wirtsprogramm oder Benutzeraktion zur Verbreitung. | Verbreitet sich eigenständig im Netzwerk. |
| Hängt sich an Programme/Dateien. | Nutzt oft Netzwerkdienste oder Schwachstellen. |

---

# 4. Ransomware

## 4.1 Definition
>**Ransomware** erpresst Lösegeld, indem sie den Zugriff auf Systeme oder Daten einschränkt bzw. Daten verschlüsselt.

Ziel ist meist eine Verletzung der **Verfügbarkeit**; moderne Varianten kombinieren dies oft mit Datenabfluss und Erpressung.

## 4.2 Klassische Ransomware
Klassische Varianten wie der „BKA-Trojaner“:

- sperren den Bildschirm,
- schränken Zugriff auf System oder Daten ein,
- fordern Lösegeld,
- verschlüsseln die Daten oft nicht tatsächlich,
- können häufig durch Start von Rettungssystemen oder Bereinigung entfernt werden.

## 4.3 Moderne Ransomware / Krypto-Trojaner

Beispiele aus den Folien: WannaCry, Locky, TeslaCrypt.

Typische Eigenschaften:
- Betriebssystem startet häufig weiterhin.
- Lokale, USB- und Netzlaufwerke können verschlüsselt werden.
- Dokumente, Fotos und viele andere Dateitypen werden betroffen.
- Originaldateien werden gelöscht.
- Teilweise werden Volumenschattenkopien entfernt.
- Lösegeld kann steigen oder Dateien werden schrittweise gelöscht.
- Zahlung häufig über Bitcoin oder Guthabenkarten.

### Unterschied klassisch vs. modern

| Klassisch | Modern / Krypto-Trojaner |
|---|---|
| Bildschirm/Benutzung gesperrt | Dateien verschlüsselt |
| Daten häufig noch intakt | Daten ohne Schlüssel oft nicht wiederherstellbar |
| Beseitigung häufig über Rettungssystem | Wiederherstellung meist über sauberes Backup nötig |
| Erpressung durch Sperre | Erpressung durch kryptographisch erzwungene Unzugänglichkeit |

## 4.4 Hybride Verschlüsselung bei Ransomware

Moderne Ransomware verwendet typischerweise **hybride Kryptographie**:

- Dateien werden mit schnellen **symmetrischen Session Keys** verschlüsselt, z. B. AES oder RC4.
- Die Session Keys werden über ein **asymmetrisches Verfahren** geschützt, z. B. RSA oder ECDH.

### Varianten

| Variante | Ablauf |
|---|---|
| **1: Schlüsselgenerierung auf infiziertem System** | Schlüsselpaar wird lokal erzeugt; Private Key wird auf Angreifer-Server hinterlegt; Public Key verschlüsselt Session Keys bzw. Daten. |
| **2: Public Key ist im Schadprogramm eingebettet** | Malware enthält bereits den Public Key des Angreifers; kein Kontakt zu einem Steuerungsserver für den Schlüsselaustausch nötig. |

Bei korrekter Implementierung ist ohne zugehörigen Private Key keine Entschlüsselung möglich.

## 4.5 Ransomware-Angriffsvektoren

- Mail-Anhänge:
  - Office-Dokumente mit Makros
  - Archive, teils passwortgeschützt
  - Programm- oder Skriptdateien
  - Dropper lädt eigentlichen Schadcode nach
- Drive-by-Downloads:
  - Umleitung per iframe oder schädlicher Werbung
  - Exploit Kit nutzt Browser-, Plug-in- oder OS-Schwachstellen
- Schwachstellen in Server-Software:
  - CMS
  - Shop-Systeme
- Nachladen über bereits infizierte Bots

## 4.6 WannaCry und EternalBlue

Die Folien nennen **EternalBlue** als Einfallstor für WannaCry.

- EternalBlue nutzte eine Schwachstelle im SMB-Protokoll.
- Nicht eingespielte Sicherheitsupdates erhöhen die Wahrscheinlichkeit erfolgreicher Angriffe massiv.
- Der Fall verdeutlicht die Bedeutung von Patchmanagement und Netzwerksegmentierung.

## 4.7 Auswirkungen auf Unternehmen

Mögliche Folgen:
- Betriebsunterbrechung
- Datenverlust
- Reputationsverlust
- Fremdschäden, wenn vertragliche Verpflichtungen nicht erfüllt werden
- bei kritischen Infrastrukturen besonders hohe Schäden
- bei einzelnen Unternehmen existenzgefährdend

## 4.8 Begünstigende Faktoren

| Versäumnis | Folge |
|---|---|
| Fehlende Awareness und technische Schutzmaßnahmen | Höhere Anfälligkeit für Spam und Phishing. |
| Fehlende Security Updates / schlechtes Patchmanagement | Drive-by- und Exploit-Angriffe sind erfolgreicher. |
| Fehlende Netzsegmentierung, große Freigaben, schwache Admin-Passwörter | Schadsoftware kann sich weiter ausbreiten; größerer Schaden. |
| Unzureichendes Backup-Konzept | Längerer bzw. irreversibler Datenverlust. |

## 4.9 Entwicklung zu gezielten Angriffen

Ransomware kann zu gezielten Erpressungen ausgebaut werden:

1. Organisation infiltrieren.
2. Daten und Backups identifizieren.
3. Verschlüsselung zeitlich koordinieren.
4. Daten kopieren (**Spionage**), löschen (**Sabotage**) oder verschlüsseln (**Erpressung**).

---

# 5. Beispiel Emotet

Emotet war ursprünglich ein Banking-Trojaner, kann aber weitere Schadfunktionen nachladen.

## Fähigkeiten laut Folien

- Verbreitung über glaubwürdig wirkende E-Mails.
- **Outlook Harvesting**:
  - E-Mail-Adressen und Kommunikationsinhalte aus Outlook/Mail-Client auslesen.
  - E-Mails auf Basis realer Kommunikationsverläufe erzeugen.
  - Dadurch wirken Phishing-Nachrichten authentisch.
- Passwörter aus System, Browser oder Mail-Client ermitteln.
- Schreibbare Windows-Fileshares suchen.
- Passwörter für Freigaben cracken.
- Weiterverbreitung im Netzwerk.
- Nachladen zusätzlicher Malware.

## Bedeutung

Emotet zeigt die Kombination mehrerer Angriffsphasen:

```text
Phishing -> Infektion -> Persistenz -> Credential Theft
-> Outlook Harvesting -> Laterale Bewegung -> Nachladen weiterer Malware
```

**Klausurpunkt:** Glaubwürdige Mails entstehen nicht nur durch gefälschte Absender, sondern oft durch kompromittierte echte Postfächer und echte Kommunikationsverläufe.

---

# 6. Human Factors

Human Factors sind menschliche Eigenschaften oder Fehler, die Angriffe ermöglichen oder erleichtern.

In den Folien:

- Spam
- Ransomware
- Social Engineering
- Scareware
- Phishing

---

## 6.1 Spam

**Spam** ist massenhaft versendete, unerwünschte Nachricht, häufig mit Werbung, Betrug oder Malware-Bezug.

Warum Spam trotz sehr niedriger Erfolgsrate wirtschaftlich sein kann:
- Versand ist extrem günstig.
- Reichweite ist sehr groß.
- Schon sehr wenige erfolgreiche Abschlüsse reichen für Profit.

Die Folie illustriert: Bei hunderten Millionen Nachrichten sind selbst wenige Käufer ein tragfähiges Geschäftsmodell.

---

## 6.2 Social Engineering

### Definition
Social Engineering sind nicht- oder niedrigtechnische Angriffsmethoden, die Täuschung, Manipulation oder Betrug nutzen, um Informationssysteme anzugreifen.

### Grundprinzip

1. Angreifer recherchiert das Umfeld:
   - Firmenwebsite
   - soziale Netzwerke
   - interne Begriffe/Slang
   - eingesetzte Systeme und Abläufe
2. Angreifer wirkt dadurch glaubwürdig.
3. Opfer gibt Informationen preis oder führt eine schädliche Handlung aus.

### Häufige Vorwände

- angeblicher Administrator
- angeblicher Vorgesetzter
- angeblicher externer Dienstleister
- angebliche IT-Sicherheit
- angebliche dringende Support-Anfrage

### Ausgenutzte menschliche Faktoren

- Hilfsbereitschaft
- Dankbarkeit
- Gutgläubigkeit
- Stress und Zeitdruck
- Respekt vor Autoritäten
- Attraktivität oder soziale Nähe
- Angst vor Konsequenzen

### Mögliche Ziele

- Passwort preisgeben
- Software installieren
- Passwort zurücksetzen
- MFA-Anfrage bestätigen
- URL öffnen
- vertrauliche Systeminformationen herausgeben

## 6.3 Beispiel Robin Sage

Robin Sage war eine künstlich geschaffene Identität in sozialen Netzwerken:

- angebliche MIT-Absolventin und IT-Sicherheitsspezialistin beim US-Militär,
- gezielte Kontaktaufnahme zu IT-Sicherheits-, Militär- und Geheimdienstpersonal,
- Glaubwürdigkeit durch viele bekannte bzw. hochrangige „Freunde“,
- dadurch Zugang zu Jobangeboten und vertraulichen Informationen.

**Lehre:** Öffentliche Profile, Beziehungen, Technologieinformationen und organisatorische Details können als Vorbereitung für Social Engineering dienen.

### Informationen, die für Angreifer wertvoll sind

- Reinigungs-/Hausmeisterdienst und Name des Dienstleisters
- eingesetztes Betriebssystem
- Versionen und Service Packs
- PDF-Software und Version
- Browser und Browserversion
- verwendeter Mail-Client
- interne Prozesse und Ansprechpartner
- URLs, die zum Öffnen verleiten können

Solche Informationen erleichtern glaubwürdige Vorwände und die Auswahl passender Exploits.

---

## 6.4 Scareware
> **Scareware** nutzt Angst und Druck.

Typischer Ablauf:
1. Programm oder Webseite meldet viele angebliche „Probleme“.
2. Die angebliche Behebung wird nur in der kostenpflichtigen Version angeboten.
3. Häufig betreffen die „Probleme“ harmlose Dinge, z. B. Cookies, Browser Cache oder History.

Ziele:
- Geldzahlung
- Installation weiterer Malware
- Erhalt von Zahlungsdaten
- Verunsicherung des Opfers

**Abgrenzung:** Scareware erzeugt vor allem psychologischen Druck; sie muss nicht zwingend technisch besonders komplex sein.

---

# 7. Phishing und verwandte Angriffe

## 7.1 Phishing
> **Phishing** ist ein Spezialfall eines Maskierungsangriffs.

Ablauf:
1. Angreifer gibt sich als legitime Organisation oder Website aus, etwa Bank oder Arbeitgeber.
2. Opfer wird meist per gefälschter E-Mail auf eine gefälschte Seite gelockt.
3. Opfer gibt Passwort, PIN, TAN oder andere Daten ein.
4. Angreifer nutzt die erlangten Daten.

Problem: Nutzer können die Authentizität von E-Mails oft nicht zuverlässig beurteilen. Digitale Signaturen für Unternehmensmails sind selten.

## 7.2 Pharming
> **Pharming** lenkt Nutzer auf eine falsche Website, obwohl sie die korrekte Adresse eingeben.

Mögliche Methoden:
- Eintrag in der `hosts`-Datei
- Manipulierter DNS-Server
- Kompromittierte Router-DNS-Einstellungen
- Drive-by-Pharming: Änderung der DNS-Konfiguration durch Schadsoftware

**Abgrenzung:**

| Phishing | Pharming |
|---|---|
| Opfer wird mit gefälschter Nachricht/Link gelockt. | Namensauflösung oder lokale Zuordnung wird manipuliert. |
| Nutzer klickt meist aktiv auf Link. | Richtige URL kann eingegeben werden, trotzdem erfolgt Umleitung. |

## 7.3 Spear Phishing
> **Spear Phishing** ist gezieltes Phishing gegen eine bestimmte Person oder Organisation.

Merkmale:
- personalisierte Ansprache
- Bezug auf reale Prozesse/Projekte
- höhere Glaubwürdigkeit
- höhere Erfolgswahrscheinlichkeit als Massenphishing

## 7.4 Fast Flux und Proxy-Infrastruktur
> Angreifer nutzen kompromittierte Rechner als Proxies:

- die eigentliche Angreiferinfrastruktur wird verschleiert,
- einzelne Hosts lassen sich schwer vollständig abschalten,
- große Anzahl verteilter Proxies erhöht Ausfallsicherheit.

**Fast-Flux-Botnets:**
- Angreifer betreiben eigenen DNS-Server.
- DNS liefert schnell wechselnde IP-Adressen zurück, oft Round Robin.
- Dadurch wird die Abschaltung bösartiger Domains/Hosts erschwert.
- „Bullet Proof Domains“ sind Domains/Hosting-Strukturen, die Abschaltung bewusst erschweren.

---

# 8. Man-in-the-Browser

## 8.1 Definition
> **Man-in-the-Browser (MitB)** ist ein Man-in-the-Middle-Angriff innerhalb des Browsers.

Meist wird er durch Trojanische Pferde umgesetzt.

## 8.2 Ablauf
1. Schadsoftware installiert Browser-Erweiterung, Skript oder Manipulationskomponente.
2. Nutzer ruft die echte, legitime Website auf.
3. Die Malware verändert die Originalseite **vor** der Anzeige im Browser.
4. Eingegebene Daten – Passwort, PIN, TAN – werden an den Angreifer übermittelt.
5. Parameter werden manipuliert, z. B. Empfängerkontonummer.
6. Antwort der echten Website wird erneut verändert, damit das Opfer keinen Verdacht schöpft.

## 8.3 Warum klassische Authentisierung allein nicht genügt

Für die echte Website wirken Anfragen authentisch:
- gleiche IP-Adresse,
- gleicher Browser,
- korrekte Zugangsdaten,
- ggf. gültiges Client-Zertifikat,
- ggf. Einmalpasswort.

Die Malware sitzt **nach** der Benutzerinteraktion, aber **vor** der Übertragung bzw. Darstellung der Daten.

## 8.4 Gegenmaßnahme: unabhängiger zweiter Kanal

Ein zweiter vertrauenswürdiger Kanal kann helfen, z. B.:
- SMS mit konkreten Überweisungsdaten,
- separate Hardware-TAN-Anzeige,
- Transaktionssignierung, bei der Empfänger und Betrag außerhalb des kompromittierten Browsers angezeigt werden.

Aber auch das kann versagen, wenn zusätzlich das Smartphone kompromittiert wird:
- **Man-in-the-Mobile**: Angriff auf mTAN/SMS oder mobile Freigaben.

---

# 9. Gegenmaßnahmen nach Angriffstyp

| Bedrohung                        | Sinnvolle Gegenmaßnahmen                                                                               |
| -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Spam/Phishing                    | Mailfilter, DMARC/SPF/DKIM, Awareness, Meldewege, MFA, sichere Prozesse.                               |
| Social Engineering               | Schulungen, Verifikation über Rückruf/zweiten Kanal, Least Privilege, klare Freigabeprozesse.          |
| Malware/Drive-by                 | Patchmanagement, Browser-Hardening, EDR/AV, Application Allowlisting, Makros restriktiv behandeln.     |
| Ransomware                       | 3-2-1-Backups, Offline/immutable Backups, Segmentierung, Patchmanagement, MFA, EDR, Incident Response. |
| ARP Poisoning                    | Netzwerksegmentierung, Dynamic ARP Inspection, sichere Switch-Konfiguration, Verschlüsselung.          |
| MitB                             | Transaktionssignierung, Gerätehärtung, separate Bestätigungskanäle, Browser-/Endpoint-Schutz.          |
| DDoS/SYN Flood                   | Rate Limiting, SYN Cookies, Load Balancing, DDoS-Schutzdienst, Kapazitätsreserven.                     |
| DNS-/Pharming-ähnliche Umleitung | DNSSEC, sichere Routerkonfiguration, Monitoring, geschützte DNS-Resolver.                              |

---

# 14. Klausur-Checkliste

Du solltest nach dem Foliensatz sicher können:
1. Die vier RFC-4949-Bedrohungsklassen nennen und erklären.
2. Exposure, Interception, Inference und Intrusion unterscheiden.
3. Masquerade, Falsification und Repudiation erklären.
4. Incapacitation, Corruption und Obstruction abgrenzen.
5. Misappropriation und Misuse unterscheiden.
6. Angriffe grob den OSI-/TCP-IP-Schichten zuordnen.
7. Virus, Wurm, Trojaner, logische Bombe, Zeitbombe und Backdoor definieren.
8. Drive-by-Download und Dropper erklären.
9. Klassische und moderne Ransomware vergleichen.
10. Hybride Verschlüsselung bei Krypto-Trojanern erklären.
11. Gründe nennen, weshalb Ransomware in Organisationen großen Schaden verursacht.
12. Emotet und Outlook Harvesting erklären.
13. Social Engineering definieren und typische psychologische Hebel nennen.
14. Scareware, Spam, Phishing, Pharming und Spear Phishing abgrenzen.
15. Fast Flux und Botnet-Proxies einordnen.
16. Man-in-the-Browser erklären und begründen, weshalb Login/MFA allein nicht immer reicht.

---

## Quellenbasis
- Foliensatz **„IT-Sicherheit 1 – Spezielle Bedrohungen“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Enthaltene Themen: RFC-4949-Bedrohungssystematik, Netzwerkangriffe, Malware, Ransomware, Emotet, Social Engineering, Scareware, Phishing, Pharming und Man-in-the-Browser.
