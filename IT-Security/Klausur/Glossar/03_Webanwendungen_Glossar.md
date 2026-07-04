# IT-Sicherheit 2 – Glossar Web-Anwendungen

> Begriffe aus der Zusammenfassung zum Foliensatz **„Sicherheit von Web-Anwendungen“**.

| Begriff | Bedeutung |
|---|---|
| **Access Control / Zugriffskontrolle** | Prüfung, ob ein bereits identifizierter Nutzer auf eine Ressource oder Funktion zugreifen darf. |
| **AJAX** | *Asynchronous JavaScript and XML*; asynchroner Datenaustausch zwischen Browser und Server ohne vollständiges Neuladen der Seite. |
| **Application Server** | Serverkomponente, die Geschäftslogik einer Webanwendung ausführt. |
| **Authentication / Authentifizierung** | Prüfung der behaupteten Identität, z. B. durch Passwort, Token oder Zertifikat. |
| **Authorization / Autorisierung** | Entscheidung, welche Aktionen oder Daten eine identifizierte Person nutzen darf. |
| **Browser Fingerprinting** | Wiedererkennung eines Browsers über Merkmale wie Version, Sprache, Fonts, Bildschirmgröße, Header oder Rendering-Verhalten. |
| **Canvas Fingerprinting** | Fingerprinting über Unterschiede beim Canvas-Rendering im Browser. |
| **CDN** | *Content Delivery Network*; verteilte Infrastruktur zur Auslieferung von Dateien wie JavaScript, CSS, Bildern oder Fonts. |
| **Clickjacking** | Täuschungsangriff mit überlagerten/unsichtbaren Frames, bei dem Nutzer auf etwas anderes klicken als sie glauben. |
| **Cookie** | Kleine Browserdatenstruktur aus Name und Wert, meist für Sessions, Einstellungen oder Tracking verwendet. |
| **Cookie Theft** | Diebstahl eines Session-Cookies, z. B. über XSS, um eine Sitzung zu übernehmen. |
| **CORS** | *Cross-Origin Resource Sharing*; servergesteuerte Ausnahme zur Same Origin Policy für Cross-Origin-Lesezugriffe. |
| **CSP** | *Content Security Policy*; Browserrichtlinie, die erlaubte Quellen und Ausführungsarten für Ressourcen festlegt. |
| **CSRF** | *Cross-Site Request Forgery*; Angriff, bei dem ein eingeloggter Browser ungewollt einen Request an eine Zielseite sendet. |
| **CSS** | *Cascading Style Sheets*; Sprache zur Gestaltung von HTML-Seiten. |
| **Data Breach** | Sicherheitsverletzung mit Verlust, Offenlegung, Veränderung oder unbefugtem Zugriff auf Daten. |
| **DNS Rebinding** | Angriff mit wechselnder DNS-Auflösung, um Browsercode gegen interne Systeme oder IP-Adressen einzusetzen. |
| **DNS Pinning** | Browser merkt sich eine IP-Adresse einer Domain zeitweise, um DNS-Rebinding zu erschweren. |
| **Domain-Attribut** | Cookie-Attribut, das bestimmt, für welche Domain(s) ein Cookie gesendet wird. |
| **DOM-based XSS** | XSS, bei dem clientseitiger JavaScript-Code untrusted Daten unsicher in das DOM einfügt. |
| **Double-Submit Cookie** | CSRF-Schutz: Token liegt im Cookie und im Formular; Server prüft beide Werte auf Gleichheit. |
| **Drive-by-Download** | Schadsoftware wird über eine kompromittierte Website bzw. ausgenutzte Browser-/Plugin-Schwachstelle ohne bewussten Download installiert. |
| **ETag Tracking** | Missbrauch von Cache-ETags zur Wiedererkennung eines Browsers. |
| **Fetch API** | Moderne JavaScript-API für asynchrone HTTP-Anfragen. |
| **First-Party Cookie** | Cookie der gerade besuchten Website. |
| **Forced Browsing** | Zugriff auf ungeschützte, aber erratbare URLs oder Ressourcen, z. B. `/admin` oder Backup-Dateien. |
| **Frame Ancestors** | CSP-Direktive, die bestimmt, welche Origins eine Seite in Frames einbetten dürfen. |
| **HSTS** | *HTTP Strict Transport Security*; zwingt Browser nach erfolgreichem HTTPS-Aufruf zur künftigen HTTPS-Nutzung. |
| **HTTP** | *Hypertext Transfer Protocol*; zustandsloses Webprotokoll für Requests und Responses. |
| **HTTPOnly** | Cookie-Attribut: JavaScript darf das Cookie nicht über `document.cookie` auslesen. |
| **HTTPS** | HTTP über TLS; schützt Übertragung durch Verschlüsselung, Integrität und üblicherweise Serverauthentisierung. |
| **iFrame** | HTML-Element zum Einbetten einer anderen Seite bzw. Ressource innerhalb einer Seite. |
| **IndexedDB** | Browserdatenbank für größere clientseitige Datenmengen. |
| **Injection** | Einschleusen untrusted Eingaben in einen Interpreter oder Ausgabekontext, z. B. SQL, Shell oder HTML/JavaScript. |
| **JavaScript Sandbox** | Sicherheitsumgebung im Browser, die JavaScript von Systemressourcen wie dem Dateisystem abschirmt. |
| **JWT** | *JSON Web Token*; signiertes Tokenformat für Claims, oft zur Sitzungs- oder API-Authentisierung genutzt. |
| **Least Privilege** | Prinzip minimaler Rechte: Nutzer, Prozesse und Dienste erhalten nur notwendige Berechtigungen. |
| **Man-in-the-Middle** | Angreifer sitzt zwischen Kommunikationspartnern und kann Daten mitlesen, verändern oder weiterleiten. |
| **Mixed Content** | Unsichere HTTP-Ressourcen innerhalb einer HTTPS-Seite; kann die Sicherheit der Seite untergraben. |
| **Multi-Tier-Architektur** | Aufteilung einer Webanwendung in Schichten wie Browser, Webserver, Anwendung und Datenbank. |
| **Nonce** | Einmaliger, kryptographisch zufälliger Wert; bei CSP zur expliziten Freigabe einzelner Inline-Skripte. |
| **Origin** | Kombination aus Protokoll, Hostname und Port. Zentral für Same Origin Policy und CORS. |
| **Output Encoding / Escaping** | Kontextabhängige Kodierung von Ausgaben, damit Eingaben nicht als HTML, JavaScript, CSS oder URL-Code interpretiert werden. |
| **OWASP** | *Open Worldwide Application Security Project*; Organisation, die u. a. die OWASP Top 10 veröffentlicht. |
| **OWASP Top 10** | Priorisierte Liste zentraler Risiken in Webanwendungen. |
| **Path-Attribut** | Cookie-Attribut, das bestimmt, für welche URL-Pfade ein Cookie gesendet wird. |
| **Persistent Cookie** | Cookie mit Ablaufdatum bzw. `Max-Age`; bleibt über Browsersitzungen hinweg gespeichert. |
| **Pharming** | Umleitung auf falsche Website durch Manipulation von DNS, Hosts-Datei oder Router-DNS-Einstellungen. |
| **Phishing** | Täuschungsangriff, bei dem Opfer über gefälschte Kommunikation auf eine falsche Website gelockt werden. |
| **Prepared Statement** | Parametrisierte Datenbankabfrage, die SQL-Code und Eingabedaten trennt und SQL Injection verhindert. |
| **Preflight Request** | CORS-Vorabprüfung mit `OPTIONS`, bevor Browser bestimmte Cross-Origin-Requests sendet. |
| **Reflected XSS** | XSS, bei dem ein schädlicher Request-Parameter in der Serverantwort zurückgespiegelt und ausgeführt wird. |
| **Referer** | HTTP-Header mit der vorher besuchten URL; kann Datenschutzprobleme verursachen. |
| **Referrer-Policy** | Richtlinie zur Steuerung, ob und wie viel Referer-Informationen bei Requests gesendet werden. |
| **Same Origin Policy (SOP)** | Browserregel, die Scriptzugriff auf Daten anderer Origins grundsätzlich beschränkt. |
| **SameSite** | Cookie-Attribut, das Cookie-Übertragung bei Cross-Site-Requests einschränkt und CSRF erschwert. |
| **SameSite=Lax** | Cookie wird in einigen Cross-Site-Navigationsfällen noch gesendet, typischerweise bei sicheren Top-Level-GETs. |
| **SameSite=Strict** | Cookie wird nur bei Same-Site-Kontext gesendet; stärkster CSRF-Schutz, aber potenziell schlechtere Usability. |
| **Sanitizing** | Bereinigung erlaubter, aber potenziell gefährlicher Eingaben wie HTML-Markup. |
| **SBOM** | *Software Bill of Materials*; Inventar aller Softwarekomponenten und Abhängigkeiten eines Produkts. |
| **Secure** | Cookie-Attribut: Cookie wird nur über HTTPS gesendet. |
| **Session** | Serverseitig verwalteter Kontext zusammengehöriger Nutzeranfragen. |
| **Session Cookie** | Cookie ohne Ablaufdatum; wird üblicherweise beim Schließen des Browsers gelöscht. |
| **Session Hijacking** | Übernahme einer Sitzung durch Diebstahl/Vorhersage einer Session-ID oder eines Session-Cookies. |
| **Session-ID** | Zufällige Kennung, mit der Server Browserrequests einer Sitzung zuordnet. |
| **Signed Double-Submit Cookie** | Double-Submit-Variante, bei der der CSRF-Token zusätzlich kryptographisch gegen Manipulation abgesichert ist. |
| **SOP** | Kurzform für Same Origin Policy. |
| **SQL Injection** | Manipulation von SQL-Abfragen durch unsicher zusammengesetzte Eingaben. |
| **Stored XSS** | Persistent gespeicherter XSS-Payload, der später bei anderen Nutzern ausgeführt wird. |
| **Subresource Integrity (SRI)** | Hashbasierte Prüfung eingebundener Drittressourcen wie JavaScript oder CSS. |
| **Third-Party Cookie** | Cookie eines eingebundenen Drittanbieters, z. B. Werbe- oder Trackingdienst. |
| **TLS** | *Transport Layer Security*; kryptographisches Protokoll für Vertraulichkeit, Integrität und Authentisierung von Verbindungen. |
| **Token / CSRF Token** | Zufälliger, nicht vorhersagbarer Wert zur Bindung eines Requests an eine legitime Sitzung. |
| **Trusted Path** | Vertrauenswürdiger, nicht durch Webinhalte manipulierbarer Kommunikationsweg zwischen Nutzer und Browser/OS. |
| **Typejacking** | Nutzung fast gleich aussehender Domainnamen zur Täuschung, z. B. `paypa1` statt `paypal`. |
| **Web Application Firewall (WAF)** | Spezialisierte Firewall für HTTP/Webanwendungen; filtert bzw. erkennt typische Webangriffe. |
| **Web Storage** | Clientseitiger Browser-Speicher, etwa `localStorage` und `sessionStorage`. |
| **WebGL Fingerprinting** | Fingerprinting über WebGL- und GPU-/Rendering-Eigenschaften. |
| **X-Frame-Options** | HTTP-Header gegen Clickjacking; regelt, ob eine Seite in Frames eingebettet werden darf. |
| **XHR** | *XMLHttpRequest*; ältere Browser-API für asynchrone HTTP-Requests. |
| **XSS** | *Cross-Site Scripting*; Einschleusen von JavaScript in eine vertrauenswürdige Website. |

## OWASP Top 10 2025 – Begriffe

| Begriff | Bedeutung |
|---|---|
| **A01 Broken Access Control** | Fehlende oder fehlerhafte Zugriffskontrolle auf Daten/Funktionen. |
| **A02 Security Misconfiguration** | Unsichere oder unvollständige Konfiguration von Komponenten. |
| **A03 Software Supply Chain Failures** | Risiken durch Abhängigkeiten, Build-, Update- oder Verteilungsprozesse. |
| **A04 Cryptographic Failures** | Unzureichender Schutz sensibler Daten durch fehlende/schwache Kryptographie. |
| **A05 Injection** | Einschleusen von Code/Befehlen in Interpreter oder Ausgabekontexte. |
| **A06 Insecure Design** | Sicherheitsproblem, das bereits aus falschen/fehlenden Designentscheidungen entsteht. |
| **A07 Authentication Failures** | Fehler bei Anmeldung, Sessionverwaltung oder Identitätsprüfung. |
| **A08 Software or Data Integrity Failures** | Fehlende Integrität in Software, Updates, Daten oder Serialisierung. |
| **A09 Security Logging and Alerting Failures** | Fehlendes oder unwirksames Logging, Monitoring, Alerting oder Incident Handling. |
| **A10 Mishandling of Exceptional Conditions** | Unsichere Behandlung von Fehlern, Grenzfällen oder Ausnahmezuständen. |
