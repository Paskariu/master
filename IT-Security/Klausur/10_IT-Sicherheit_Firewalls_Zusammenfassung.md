# IT-Sicherheit – Firewalls

> **Foliensatz:** `ITsecFR-2b Firewalls.pdf`  
> **Dozent:** Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe  
> **Klausurfokus:** Zweck und Grenzen von Firewalls, Architekturvarianten, Paketfilter, NAT, Proxies, DMZ und SOCKS.

---

# 1. Grundprinzip einer Firewall

Eine Firewall kontrolliert Kommunikationsübergänge zwischen Sicherheitszonen. Sie lässt nur Verbindungen bzw. Pakete zu, die durch Regeln autorisiert sind.

Typische Ziele:

- unerwünschte Zugriffe von außen blockieren,
- ausgehende Kommunikation kontrollieren,
- interne Netzstruktur verbergen,
- Dienste in getrennten Zonen betreiben,
- Protokollierung und Filterung zentralisieren.

## Zentrale Voraussetzung

Eine Firewall kann nur schützen, wenn **alle relevanten Datenpakete sie passieren**.

Umgehungen:

- zweiter Internetanschluss,
- mobiles Internet,
- WLAN,
- externe Fernwartung,
- kompromittierte interne Systeme.

**Merksatz:** Eine Firewall schützt den kontrollierten Übergang – nicht automatisch das gesamte Unternehmen.

---

# 2. Firewall-Strukturen

## 2.1 Lokale Firewall

Eine lokale Firewall läuft direkt auf einem Endgerät oder Server.

Vorteile:

- Schutz auch innerhalb des Netzes,
- Teil eines Defense-in-Depth- bzw. Zwiebelschalenmodells,
- kann Regeln pro Host/Prozess durchsetzen.

Nachteil:

- muss auf vielen Systemen gepflegt werden,
- schützt nicht gegen Fehlkonfigurationen oder kompromittierte Hosts.

---

## 2.2 Screened Host

Ein **Screened Host** ist eine einfache Firewall-Architektur.

Prinzip:

```text
Internet -> Paketfilter/Firewall -> interner Server bzw. internes Netz
```

Eigenschaften:

- einfache und weit verbreitete Konstruktion,
- Server kann exponiert erreichbar sein,
- kein ausreichender Schutz, wenn der exponierte Server kompromittiert wird.

**Klausurpunkt:** Der kompromittierte Server kann als Sprungbrett ins interne Netz dienen, wenn Trennung/Regeln unzureichend sind.

---

## 2.3 DMZ

Eine **DMZ – Demilitarized Zone** ist eine separate Sicherheitszone für öffentlich erreichbare Dienste.

Typische Dienste:

- Webserver,
- Mail Relay,
- Reverse Proxy,
- DNS-Server.

Prinzip:

```text
Internet <-> DMZ <-> internes Netz
```

Merkmale:

- öffentliche Dienste liegen nicht direkt im internen Netz,
- Kommunikation zwischen Internet, DMZ und internem Netz wird getrennt geregelt,
- häufig mit einer Firewall und drei Netzwerkports realisiert.

Beispiel aus der Grafik auf Folie 6:

```text
Outside-Netz
    |
 Firewall
 /       \
DMZ      Inside-Netz
```

Dadurch können z. B. externe Nutzer auf den DMZ-Webserver zugreifen, ohne direkten Zugriff auf interne Hosts zu bekommen.

---

## 2.4 Screened Subnet / doppelte Firewall mit DMZ

Ein **Screened Subnet** verwendet zwei getrennte Firewall-Grenzen:

```text
Internet -> äußere Firewall -> DMZ -> innere Firewall -> internes Netz
```

Vorteile:

- stärkere Segmentierung,
- Kompromittierung eines DMZ-Systems führt nicht automatisch zu internem Zugriff,
- mehrere Schutzschichten.

Nachteil:

- höhere Komplexität,
- mehr Regeln und Betriebsaufwand.

---

## 2.5 Bastion Host

Ein **Bastion Host** ist ein besonders gehärtetes, exponiertes System.

Typische Eigenschaften:

- minimal installierte Software,
- restriktive Konfiguration,
- zentrale Protokollierung,
- erhöhte Überwachung,
- nur notwendige Dienste geöffnet.

Er wird häufig in oder nahe einer DMZ betrieben.

---

## 2.6 Strikte Proxy-Architektur

Die Folien beschreiben eine sehr sichere, aber unflexible Struktur:

- keine direkte Kommunikation zwischen intern und extern,
- Kommunikation erfolgt nur über Vermittler/Proxies,
- teilweise ohne klassischen Paketfilter.

Vorteil:

- starke Trennung,
- weniger direkte Angriffsfläche.

Nachteil:

- unflexibel,
- höherer Verwaltungsaufwand,
- moderne Anwendungen lassen sich nicht immer einfach proxyen.

---

# 3. Firewall-Funktionen

| Art | Ebene / Funktion | Beispiele |
|---|---|---|
| **Paketfilter** | Filtert einzelne Pakete anhand technischer Merkmale. | ACLs, Router-Regeln |
| **Stateful Paketfilter** | Berücksichtigt Verbindungszustände. | TCP-Verbindungsaufbau/Rückverkehr |
| **Circuit-Level Proxy** | Vermittelt Verbindungen, ohne Anwendungsprotokoll tief zu verstehen. | SOCKS |
| **Application-Level Proxy** | Prüft/vermittelt anwendungsspezifisch. | HTTP-/WWW-Proxy |
| **NAT** | Übersetzt/versteckt interne Adressen. | Private interne Netze hinter öffentlicher Adresse |

---

# 4. Paketfilter und ACLs

## 4.1 ACL

**ACL – Access Control List** ist eine Regelmenge, die an Netzwerkinterfaces angewendet wird.

Regeln können gelten:

- inbound: eingehender Verkehr,
- outbound: ausgehender Verkehr.

Typische Filterkriterien:

- Quell-IP-Adresse,
- Ziel-IP-Adresse,
- Protokoll,
- Quellport,
- Zielport,
- Richtung,
- Verbindungszustand.

## 4.2 Beispiele aus dem Foliensatz

```text
Permit 10.1.1.0 0.0.0.255
```

Erlaubt ein bestimmtes Netz bzw. Adressbereich.

```text
Permit ip any 10.1.1.0 0.0.0.255
```

Erlaubt IP-Verkehr zu diesem Zielnetz.

```text
Permit tcp host 10.1.1.0 host 172.16.1.1 eq telnet
```

Erlaubt TCP von einem bestimmten Host zu einem bestimmten Zielhost auf Telnet-Port.

## 4.3 Statischer und dynamischer Filter

| Typ | Bedeutung |
|---|---|
| **Statisch** | Jede Regel wird unabhängig vom Verbindungszustand geprüft. |
| **Dynamisch / Stateful** | Firewall merkt sich Verbindungszustände, z. B. TCP-Session, und erlaubt passenden Rückverkehr. |

Stateful Filtering ist besonders relevant, weil Antworten auf intern initiierte Verbindungen nicht vollständig als separate statische Regeln modelliert werden müssen.

## 4.4 NAT

**NAT – Network Address Translation** übersetzt Adressen zwischen internen und externen Netzen.

Nutzen:

- interne Adressstruktur bleibt nach außen verborgen,
- mehrere interne Systeme können über wenige öffentliche IP-Adressen kommunizieren.

Grenze:

- NAT ist kein vollständiger Sicherheitsmechanismus,
- es ersetzt keine Firewall-Regeln,
- Anwendungen/Protokolle mit eingebetteten IP-Adressen können problematisch sein.

---

# 5. Proxy-Firewalls

## 5.1 Grundidee

Ein Proxy vermittelt Kommunikation statt direkte Verbindung zwischen internem Client und externem Ziel.

```text
Client -> Proxy -> Internetdienst
```

Vorteile:

- interne Struktur wird verborgen,
- zentrale Kontrolle,
- Protokollierung,
- ggf. Inhaltsprüfung.

---

## 5.2 Circuit-Level Proxy

Ein Circuit-Level Proxy arbeitet auf Verbindungsebene.

Typisches Beispiel: **SOCKS**.

Eigenschaften:

- Nutzer kann ihn oft transparent nutzen,
- Proxy vermittelt Verbindungsaufbau,
- interne IP-Struktur bleibt verborgen,
- weniger Protokollverständnis als Application-Level Proxy.

---

## 5.3 Application-Level Proxy / WWW-Proxy

Ein Application-Level Proxy versteht ein konkretes Anwendungsprotokoll, z. B. HTTP.

Aufgaben eines WWW-Proxies laut Foliensatz:

- interne Struktur verbergen,
- Cache für häufig abgerufene Seiten,
- weniger Datenverkehr am Internetanschluss,
- zentrale Malware-Filterung,
- Sperren bestimmter Seiten,
- Protokollierung aller Seitenabrufe.

### Trade-off

| Vorteil | Nachteil |
|---|---|
| zentrale Filterung und Malware-Schutz | umfangreiche Nutzungsprotokollierung kann Privacy beeinträchtigen |
| Caching spart Bandbreite | HTTPS-Inspection ist komplex und kann Ende-zu-Ende-Vertrauen brechen |
| zentrale Sperrlisten möglich | Sperren können ungewollt legitime Nutzung einschränken |

---

# 6. SOCKS-Proxy

## 6.1 Ablauf

Ein SOCKS-Proxy arbeitet grob in drei Phasen:

1. Authentifizierung,
2. Verbindungsanfrage,
3. Datenaustausch.

## 6.2 SOCKS5-Verbindungsanfrage

In den Folien:

| Feld | Bedeutung |
|---|---|
| `VER` | Protokollversion, bei SOCKS5 `0x05`. |
| `CMD` | gewünschte Operation. |
| `ATYP` | Adresstyp. |
| `REP` | Antwortstatus des Servers. |

### CMD-Werte

| Wert | Bedeutung |
|---|---|
| `1` | TCP aktiv: `CONNECT` |
| `2` | TCP passiv: `BIND` |
| `3` | UDP |

### ATYP-Werte

| Wert | Bedeutung |
|---|---|
| `1` | IPv4-Adresse |
| `3` | DNS-Domainname |
| `4` | IPv6-Adresse |

### REP-Beispiele

| Wert | Bedeutung |
|---|---|
| `1` | Verbindung erfolgreich |
| `4` | Host nicht erreichbar |

---

# 7. Grenzen von Firewalls

Eine Firewall ist kein vollständiger Schutzmechanismus.

## 7.1 Kein Schutz vor internen Angriffen

Wenn ein Angreifer bereits im internen Netz ist, greift der Perimeterschutz nicht oder nur begrenzt.

Beispiele:

- kompromittierter Mitarbeiter-PC,
- Insider,
- infiziertes Gerät im WLAN,
- unsichere interne Freigaben.

## 7.2 Trojaner über erlaubte Ports

Ein Trojaner kann ausgehende Verbindungen über erlaubte Ports nutzen, z. B.:

```text
HTTP/HTTPS über Port 80/443
```

Dadurch kann er Firewall-Regeln umgehen, wenn nur Portfreigaben statt inhaltlicher/kontextbezogener Prüfung verwendet werden.

## 7.3 Regelaufblähung

Mit zunehmender Nutzungsdauer entstehen oft immer mehr Ausnahmen:

```text
mehr Anwendungen -> mehr Ports/Freigaben -> größere Angriffsfläche
```

Das reduziert die Wirksamkeit einer ursprünglich restriktiven Firewall.

## 7.4 Sicherheit vs. Bequemlichkeit

Es besteht ein dauerhafter Trade-off:

| Mehr Sicherheit | Mehr Bequemlichkeit |
|---|---|
| restriktive Regeln | viele Freigaben |
| Proxy-Zwang | direkte Verbindungen |
| starke Segmentierung | einfache Kommunikation |
| detaillierte Kontrolle | geringe administrative Hürden |

Fernwartungs- und Remote-Access-Tools können diesen Zielkonflikt verschärfen, wenn sie Kommunikationswege eröffnen, die klassische Netzgrenzen umgehen.

---

# 8. Best Practices

- **Default Deny:** Was nicht explizit erlaubt ist, wird blockiert.
- Netz segmentieren: intern, DMZ, externe Zone, kritische Systeme.
- DMZ-Dienste nicht direkt mit internen Datenbanken/Netzen koppeln.
- Regeln minimal und nachvollziehbar halten.
- Regeln regelmäßig prüfen und alte Freigaben entfernen.
- Inbound und Outbound kontrollieren.
- Logging und Monitoring aktivieren.
- Host-Firewalls zusätzlich zum Perimeterschutz nutzen.
- Remote-Zugänge bewusst absichern und dokumentieren.
- Firewalls mit Patchmanagement, IDS/IPS, Endpoint-Schutz und sicheren Anwendungen kombinieren.

---

# 9. Zentrale Abgrenzungen

| Begriffe | Unterschied |
|---|---|
| **Paketfilter / Proxy** | Paketfilter prüft Paketmerkmale; Proxy vermittelt Kommunikation und kann tiefer prüfen. |
| **Circuit-Level / Application-Level Proxy** | Circuit-Level vermittelt Verbindungen; Application-Level versteht konkretes Protokoll wie HTTP. |
| **DMZ / internes Netz** | DMZ enthält exponierte Dienste; internes Netz enthält schützenswerte interne Systeme. |
| **Screened Host / Screened Subnet** | Screened Host = einfache Struktur; Screened Subnet = DMZ zwischen zwei Schutzgrenzen. |
| **NAT / Firewall** | NAT übersetzt Adressen; Firewall erzwingt Zugriffsregeln. NAT ist kein vollständiger Ersatz für Firewalling. |
| **Statischer / stateful Paketfilter** | Statisch prüft jedes Paket isoliert; stateful berücksichtigt Verbindungszustand. |
| **Inbound / Outbound** | Inbound: Verkehr zum geschützten Netz; Outbound: Verkehr aus dem geschützten Netz. |

---

# 10. Klausur-Checkliste

Du solltest erklären können:

1. Welche Voraussetzung erfüllt sein muss, damit eine Firewall wirksam schützt.
2. Warum mobiles Internet, WLAN oder zweite Anschlüsse Firewall-Schutz umgehen können.
3. Lokale Firewall, Screened Host, DMZ, Screened Subnet und Bastion Host vergleichen.
4. Warum eine DMZ öffentliche Dienste vom internen Netz trennt.
5. Paketfilter und ACLs erklären.
6. Nach welchen Kriterien eine ACL filtern kann.
7. Statisches und stateful Filtering abgrenzen.
8. NAT und seine Grenzen erklären.
9. Circuit-Level Proxy, SOCKS und Application-Level Proxy vergleichen.
10. Aufgaben und Datenschutzrisiken eines WWW-Proxies nennen.
11. SOCKS5-Phasen und CMD/ATYP/REP grob einordnen.
12. Warum eine Firewall nicht gegen interne Angriffe schützt.
13. Wie Trojaner offene ausgehende Ports nutzen können.
14. Warum Firewall-Regeln mit der Zeit unsicherer werden können.
15. Warum Defense in Depth nötig ist.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit – Firewalls“**, Prof. Dr. Johannes Freudenmann, DHBW Karlsruhe.
- Themen: Firewall-Strukturen, Paketfilter/ACLs, NAT, DMZ, Proxies und Grenzen von Perimeterschutz.
