# IT-Sicherheit – Verschlüsselung: Abkürzungen und Begriffe

> Aus der Zusammenfassung zum Foliensatz **„IT-Sicherheit – Verschlüsselung“**.

## Abkürzungen

| Kürzel | Bedeutung | Kurzbeschreibung |
|---|---|---|
| **A3** | GSM Authentication Algorithm | GSM-Algorithmus zur Berechnung der Authentisierungsantwort. |
| **A5** | GSM Ciphering Algorithm | GSM-Algorithmus zur Verschlüsselung der Funkdaten. |
| **A8** | GSM Key Generation Algorithm | GSM-Algorithmus zur Ableitung des Sitzungsschlüssels `Kc`. |
| **AES** | Advanced Encryption Standard | Symmetrisches Verschlüsselungsverfahren. |
| **AH** | Authentication Header | IPsec-Protokoll für Integrität, Authentizität und Replay-Schutz ohne Vertraulichkeit. |
| **AP** | Access Point | WLAN-Basisstation. |
| **AuC / AUC** | Authentication Center | Authentisierungszentrum im Mobilfunknetz. |
| **AUTN** | Authentication Token | UMTS-Authentisierungsnachweis des Netzes. |
| **BTS** | Base Transceiver Station | Funkbasisstation im GSM-Netz. |
| **CBC** | Cipher Block Chaining | Blockchiffre-Betriebsmodus; bei TLS 1.3 ausgeschlossen. |
| **CK** | Cipher Key | UMTS-Schlüssel für Verschlüsselung. |
| **CRC** | Cyclic Redundancy Check | Prüfsumme für Zufallsfehler, kein kryptographischer Integritätsschutz. |
| **DES / 3DES** | Data Encryption Standard / Triple DES | Alte symmetrische Verfahren, in TLS 1.3 ausgeschlossen. |
| **DH** | Diffie-Hellman | Schlüsselaustauschverfahren. |
| **DSS** | Digital Signature Standard | Signaturstandard; im Foliensatz als Absicherung von DH genannt. |
| **ECDHE** | Ephemeral Elliptic Curve Diffie-Hellman | Ephemerer DH-Schlüsselaustausch auf elliptischen Kurven. |
| **ECDSA** | Elliptic Curve Digital Signature Algorithm | Signaturverfahren auf elliptischen Kurven. |
| **EAP** | Extensible Authentication Protocol | Framework für Netzwerk-/Enterprise-Authentisierung. |
| **ESP** | Encapsulating Security Payload | IPsec-Protokoll für Vertraulichkeit und optional Integrität/Authentizität. |
| **F8 / F9** | UMTS Sicherheitsfunktionen | F8 erzeugt Verschlüsselungs-Stream; F9 berechnet Integritäts-MAC. |
| **GCM** | Galois/Counter Mode | AEAD-Modus für AES, z. B. AES-GCM. |
| **GSM** | Global System for Mobile Communications | Mobilfunkstandard der zweiten Generation. |
| **HMAC** | Hash-based Message Authentication Code | Schlüsselabhängiger Integritäts-/Authentizitätsprüfwert. |
| **HTTP** | Hypertext Transfer Protocol | Webprotokoll. |
| **HTTPS** | HTTP over TLS | HTTP über TLS-geschützte Verbindung. |
| **ICV** | Integrity Check Value | Integritätsprüfwert; fachlich verwandt mit MAC. |
| **IKE / IKEv2** | Internet Key Exchange / Version 2 | Schlüsselmanagement für IPsec. |
| **IK** | Integrity Key | UMTS-Schlüssel für Integritätsschutz. |
| **IMSI** | International Mobile Subscriber Identity | Dauerhafte Teilnehmerkennung im Mobilfunk. |
| **IPsec** | Internet Protocol Security | Sicherheitsarchitektur auf IP-/Netzwerkschicht. |
| **IV** | Initialization Vector | Initialisierungswert, z. B. bei WEP. |
| **Kc** | Ciphering Key | GSM-Sitzungsschlüssel für A5. |
| **Ki** | Individual Subscriber Authentication Key | Individueller geheimer Teilnehmer-Schlüssel in GSM. |
| **MAC** | Message Authentication Code | Kryptographischer Prüfwerta für Integrität und Authentizität. |
| **MIC** | Message Integrity Check | Integritätsmechanismus in WPA/TKIP. |
| **MSC** | Mobile Switching Center | Vermittlungskomponente im GSM-Mobilfunknetz. |
| **PFS** | Perfect Forward Secrecy | Spätere Kompromittierung von Langzeitschlüsseln soll alte Sitzungen nicht offenlegen. |
| **PKDF** | Password-Based Key Derivation Function | Ableitung eines Schlüssels aus Passphrase/SSID/Parametern. |
| **PMK** | Pairwise Master Key | WPA-Schlüsselbasis, aus Passphrase/PSK abgeleitet. |
| **PSK** | Pre-Shared Key | Vorab geteiltes Geheimnis. |
| **PTK** | Pairwise Transient Key | Dynamisch abgeleiteter WPA-Schlüssel für konkrete Verbindung. |
| **QoS** | Quality of Service | Priorisierung und Steuerung von Netzwerkverkehr. |
| **RADIUS** | Remote Authentication Dial-In User Service | Dienst/Protokoll für zentrale Netzwerk-Authentisierung. |
| **RAND** | Random Challenge | Zufallswert in GSM/UMTS-Authentisierung. |
| **RC4** | Rivest Cipher 4 | Veraltete Stromchiffre, Grundlage von WEP und WPA/TKIP. |
| **RES / XRES** | Response / Expected Response | UMTS-Antwort des Geräts bzw. erwartete Antwort des Netzes. |
| **RFC** | Request for Comments | Reihe technischer Internetstandards. |
| **RNC** | Radio Network Controller | Steuerungskomponente im UMTS-Funknetz. |
| **SA** | Security Association | IPsec-Parameterzustand einer Richtung. |
| **SAE** | Simultaneous Authentication of Equals | WPA3-Authentisierungsverfahren auf PSK-Basis. |
| **SRES** | Signed Response | GSM-Authentisierungsantwort. |
| **SPD** | Security Policy Database | IPsec-Regelwerk für Schutzentscheidungen. |
| **SPI** | Security Parameters Index | Kennung eines IPsec-Pakets zur Zuordnung der SA beim Empfänger. |
| **SSH** | Secure Shell | Sicheres Remote-Login- und Tunneling-Protokoll. |
| **SSL** | Secure Sockets Layer | Vorgänger von TLS; veraltet. |
| **TCP** | Transmission Control Protocol | Verbindungsorientiertes Transportprotokoll. |
| **TMSI** | Temporary Mobile Subscriber Identity | Temporäre GSM-Teilnehmerkennung. |
| **TKIP** | Temporal Key Integrity Protocol | WPA-Übergangsmechanismus mit RC4, MIC und dynamischen Schlüsseln. |
| **TLS** | Transport Layer Security | Standardprotokoll für kryptographisch geschützte Client-Server-Verbindungen. |
| **UMTS** | Universal Mobile Telecommunications System | Mobilfunkstandard der dritten Generation. |
| **USIM** | Universal Subscriber Identity Module | UMTS-Variante des Teilnehmermoduls. |
| **VPN** | Virtual Private Network | Geschütztes virtuelles Netz über öffentliche Infrastruktur. |
| **WEP** | Wired Equivalent Privacy | Obsoletes WLAN-Sicherheitsverfahren. |
| **WPA** | Wi-Fi Protected Access | WLAN-Sicherheitsstandard als Übergang von WEP. |

## Zentrale Begriffe

| Begriff | Bedeutung |
|---|---|
| **4-Way Handshake** | WPA/WPA2-Protokoll zur Ableitung/Installation gemeinsamer Verbindungsschlüssel. |
| **AEAD** | Verschlüsselung mit integrierter Authentizität/Integrität, z. B. AES-GCM. |
| **Cipher Suite** | Festgelegte Kombination aus Schlüsselaustausch, Authentisierung, Verschlüsselung und Hash/MAC. |
| **Client Authentication** | TLS-Authentisierung des Clients durch Zertifikat und Signatur. |
| **Forward Secrecy** | Siehe PFS. |
| **Key Reinstallation Attack** | Wiederinstallation eines Schlüssels, wodurch Zähler/Nonce-Zustand problematisch zurückgesetzt wird. |
| **Master Secret** | Gemeinsame TLS-Geheimbasis, aus der Verbindungsschlüssel abgeleitet werden. |
| **Nonce** | Wert, der pro Protokollschritt/Sitzung nur einmal verwendet werden soll. |
| **Replay-Angriff** | Wiederholung zuvor gültiger Nachrichten/Pakete. |
| **Session** | In TLS Aushandlung gemeinsamer Parameter; kann mehrere TCP-Verbindungen umfassen. |
| **Transport Mode** | IPsec-Modus: schützt hauptsächlich Nutzlast. |
| **Tunnel Mode** | IPsec-Modus: kapselt gesamten ursprünglichen IP-Header und Nutzlast. |

## Häufige Verwechslungsgefahr

| Begriffe | Unterschied |
|---|---|
| **WPA / WPA2 / WPA3** | WPA = Übergang mit TKIP/RC4; WPA2 = AES; WPA3 = SAE, bessere Handshakes und PFS. |
| **GSM / UMTS** | GSM nutzt A3/A5/A8; UMTS ergänzt gegenseitige Authentisierung, CK/IK und Integritätsschutz. |
| **AH / ESP** | AH schützt Integrität/Auth., ESP liefert Vertraulichkeit plus optional Integrität/Auth. |
| **SPD / SA / SPI** | SPD entscheidet per Regeln; SA enthält Parameter; SPI referenziert beim Empfang die SA. |
| **TLS / SSH** | TLS ist allgemeiner Transport-/Client-Server-Schutz; SSH ist Anwendungsschicht für Remote-Zugriff und Tunneling. |
| **Local / Remote Forwarding** | Local leitet lokalen Port zu Remoteziel; Remote leitet Port am SSH-Server zum lokalen Ziel. |
| **Session / TCP-Verbindung** | TLS Session kann mehrere Verbindungen umfassen; Verbindung ist einzelne TCP-Verbindung mit eigenen abgeleiteten Keys. |
