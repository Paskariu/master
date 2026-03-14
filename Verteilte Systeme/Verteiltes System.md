#Klausurrelevant #TODO
# Definition
>Ein Verteiltes System ist eine Kollektion unabhängiger Computer, die den Benutzern als ein Einzelcomputer erscheinen.

Ein Verteiltes System ist ein System in dem
+ Hard- und Softwarekomponenten    
+ die sich auf miteinander vernetzten Computern befinden
+ miteinander kommunizieren und ihre Aktionen koordinieren
+ indem sie Nachrichten austauschen

+ besteht aus einer Menge autonomer Computer (auch Komponenten)
+ die durch ein Computernetzwerk (Kommunikationsbus) miteinander verbunden sind
+ und mit einer Software zur Koordination ausgestattet sind
+ einheitliches Gesamtsystem
+ Ressourcen wie Hardware, Software & Daten

>Eine **verteilite Anwendung** ist eine Anwendung, die ein Verteiltes System zur Lösung eines Anwendungsproblems nutzt. Sie besteht aus verschiedenen Softwarekomponenten, die mit den Komponenten des VS sowie den Anwendern kommuniziert.

# Anwendung
+ Per definition verteilte Anwendungen z.B. Messengerapplikationen
+ bessere Verfügbarkeit und Fehlertoleranz
	+ Redundanz von Hard- und Software
+ Performance und Leistung
	+ Rechenleistungsverteilung nach Anwendung
	+ Verteilung über viele Rechenr
+ Lösen von zu großen Problemen wie KI Trainierung und BigData

# Probleme
+ Kommunikationsversagen z.B. ein Konten ist ausgefallen
+ Rechenprozesse können ohne Kenntnis ausfallen
+ Nicht-deterministische Fehler
+ Probleme bei Entwicklug
	+ Parallele Aktivitäten
	+ globale Zeit schwer erfassbar
	+ kein konsistente Sicht des Gesamtzustandes
	+ zeit und räumliche Trennung von  Ursache und Wirkung
+ Räumliche Separation von Autonomen Komponenten
+ Heterogenität
+ Dynamik & Offenheit
+ hohe Komplexität
+ Sicherheitsgewährleistung
+ Probleme bei sequenziellen Systemen
+ Nebenläufigkeit
+ Nichtdeterminismus
+ Zustandsverteilung
+ Schwere Synchronisation
+ komplexe Programmierung
+ aufwendiges Testen

## 8 falsche Annahmen zu Verteilten Systemen
#TODO Seite 15


# Latenz
+ hohe Latenz bei topologischer Trennung
+ Bearbeitungszeit von Router, Switch etc.
+ Örtlichkeit des Clients
+ [[Content Delivery Network]]
## Synchrone Verteilte Systeme
>System mit einer Menge von Prozessoren, die über einen internen Kommunikationsbus miteinander Verknüpft sind, und eine gemeinsame Uhr haben z.B. Supercomputer oder Mehrprozessorsystem
+ Übertragung der Nachrichten brauchen nur begrenzte Zeit
+ Beschränkte Zeitliche Verschiebung der Uhren
+ [[Untere Schranke & Obere Schranke|lb]] < ptime < [[Untere Schranke & Obere Schranke|ub]]
	+ ptime = Ausführungszeit
# Asynchrone Verteilte Systeme
+ Latenz ist beliebig hoch
+ zeitliche Verschiebung der Systemuhren ist beliebig hoch
+ Bearbeitungsvorgang kann beliebig lange dauern
+ Protokolle für asynchrone Systeme funktionieren auch für synchrone, aber nicht andersrum
### Beispiele
+ verteilite Internetanwendungen
+ Ad-Hoc Netzwerkanwendungen
+ Sensornetzwerke
+ IoT
# Partielle Synchrone Verteilte Systeme
+ asynchrone Systeme mit lokalen Uhren, deren Zeit nicht beliebig auseinander driftet
+ Latenz und Verarbeitung nur in Ausnahmefällen beliebig hoch
	+ client und serverseitiger Timeout
+ Modell für reale verteilte Systeme

## Zentralle Begriffe und Eigenschaften
> **Richtige** Verarbeitung in **endlicher** Zeit

+ **Korrektheit**
	+ Im Sinne der Applikationssemantik in jeder Bearbeitungssituation "richtiges Verhalten"
+ **Lebendigkeit**
	+ zeitnahe Ausführung von notwendigen Aktionen durch das System
+ Ohne ist die Umsetzung eines verteilten Systems nicht möglich
## QoL
+ Performanz
+ UX
+ Verfügbarkeit
+ Sicherheit
+ Skalierbarkeit
+ Zuverlässigkeit

SEITE 30
