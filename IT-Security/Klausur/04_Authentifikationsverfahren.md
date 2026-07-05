# IT-Sicherheit 3 – Authentifikationsverfahren

> **Foliensatz:** `3_authentifikation_v9d.pdf`  
> **Klausurfokus:** Begriffe trennen, Faktoren/Verfahren vergleichen, Passwortspeicherung, OTP/Challenge-Response, FIDO2/Passkeys und SSO mit Shibboleth, SAML, OpenID/OIDC erklären.

---

# 1. Grundbegriffe

## 1.1 Zweck der Authentifikation

Zum Schutz gegen Maskierung und unautorisierten Zugriff müssen sicherheitsrelevante Subjekte und Objekte eindeutig identifizierbar sein. Ein Subjekt weist seine behauptete Identität durch **Credentials** nach, z. B. Benutzername + Passwort oder Zertifikat.

Für kritische Aktionen sollte eine erneute Authentifikation erfolgen, z. B.:
- Installation von Software
- Passwortänderung
- einzelne Überweisung im Online-Banking

**Beidseitige Authentifikation** ist wichtig: Nicht nur der Nutzer muss sich gegenüber dem System ausweisen; auch das System sollte sich gegenüber dem Nutzer authentisieren. Sonst sind Maskierungs-/Phishing-Angriffe möglich.

## 1.2 Begriffe sauber abgrenzen

| Begriff | Bedeutung |
|---|---|
| **Identität** | Summe von Merkmalen, die eine Unterscheidung erlauben. Kann statisch sein, z. B. Name/Steuer-ID, oder dynamisch, z. B. IP-Adresse. |
| **Identifizierung** | Mitteilung einer Identität an ein System, z. B. Benutzername, Chipkarte oder biometrisches Merkmal. |
| **Authentifizierung** | Überprüfung, ob die behauptete Identität echt ist. |
| **Autorisierung** | Zuweisung/Prüfung von Zugriffsrechten auf Ressourcen. |
| **Zugriffskontrolle** | Durchsetzung und Überwachung von Zugriffen auf Objekte/Ressourcen. |
| **Credential** | Authentifikationsnachweis, z. B. Passwort, Zertifikat, Token oder biometrisches Template. |

**Merksatz:**  
Identifizierung = „Ich bin Alice.“  
Authentifizierung = „Beweise, dass du Alice bist.“  
Autorisierung = „Darf Alice diese Aktion ausführen?“

---

# 2. Authentifikationsfaktoren und MFA

## 2.1 Faktoren

| Faktor | Frage | Beispiele |
|---|---|---|
| **Wissen** | Was weißt du? | Passwort, PIN, Sicherheitsfrage |
| **Besitz** | Was hast du? | Chipkarte, Hardware-Token, Smartphone |
| **Biometrie** | Was bist du? | Fingerabdruck, Gesicht, Iris, Stimme |

## 2.2 Multi-Factor Authentication

**MFA / 2FA / 3FA** kombiniert unabhängige Faktoren, z. B.:

```text
Chipkarte + PIN
Passwort + OTP-App
Passkey/Token + biometrische Entsperrung
```

Mehrere Geheimnisse desselben Faktors sind **keine** MFA, z. B. Passwort + Sicherheitsfrage.

MFA reduziert Schäden durch kompromittierte Einzel-Faktoren, schützt aber nicht automatisch gegen Phishing/MITM, wenn der Angreifer die Authentisierung in Echtzeit weiterleitet.

---

# 3. Passwörter

## 3.1 Passwortauthentisierung

Klassisch:

```text
Benutzername = Identifizierung
Passwort = Authentifizierung
```

Typische Schwächen:
- Wiederverwendung auf mehreren Diensten
- schlechte Usability
- Phishing
- Social Engineering / Shoulder Surfing
- schwache oder unsichere Speicherung
- Online- und Offline-Cracking

## 3.2 Passwort-Best-Practices aus dem Foliensatz
- Länge wichtiger als künstliche Komplexitätsregeln.
- Unsichere/kompromittierte Passwörter über Deny List ablehnen.
- Passwörter nicht zwischen Diensten wiederverwenden.
- Keine erzwungenen periodischen Passwortwechsel ohne Anlass.
- Copy & Paste zulassen, damit Passwortmanager nutzbar sind.
- Fehlversuche drosseln, verzögern oder sperren.
- Keine Passwort-Hinweise.
- MFA einsetzen.
- Nutzer schulen.

## 3.3 Angriffe auf Passwörter

| Angriff | Erklärung | Gegenmaßnahmen |
|---|---|---|
| **Phishing / Social Engineering** | Nutzer wird zur Preisgabe gebracht. | MFA, Nutzertraining, Systemauthentisierung, phishing-resistente Verfahren wie FIDO2. |
| **Shoulder Surfing** | Beobachten der Eingabe. | Sichtschutz, MFA, Geräte-/Umgebungsschutz. |
| **Online Brute Force** | Viele Loginversuche gegen Formular/API. | Rate Limiting, Wartezeiten, CAPTCHA, IP-/Account-Sperren, Logging. |
| **Offline Cracking** | Angreifer besitzt Passwort-Hashes und probiert lokal unbegrenzt. | Salt, Pepper, langsame/memory-hard Passwort-Hashverfahren. |
| **Wörterbuchangriff** | Kandidaten aus Listen, Regeln und bekannten Leaks. | Lange einzigartige Passphrasen, Deny Lists. |
| **Rainbow Tables** | Vorberechnete Hash-Ketten für häufige Passwörter. | Individueller Salt pro Passwort. |

## 3.4 Sichere Passwortspeicherung

Passwörter dürfen **nie im Klartext** gespeichert werden.

Stattdessen:

```text
Hash = PasswordHash(password, individueller Salt, Parameter)
```

Datenbank speichert typischerweise:

```text
UserID | Salt | Hashwert | Algorithmus-/Parameterinformationen
```

### Salt

Ein **Salt** ist ein zufälliger, individueller Wert pro Nutzer/Passwort.

Wirkung:

- gleiche Passwörter erzeugen unterschiedliche Hashes,
- vorberechnete Rainbow Tables werden praktisch unbrauchbar,
- Massenangriffe auf eine Datenbank werden teurer.

Salt ist **nicht geheim** und wird zusammen mit dem Hash gespeichert.

### Pepper

Ein **Pepper** ist ein zusätzliches systemweites Geheimnis.

Beispiele:

```text
h(salt || password || pepper)
h(pepper || hash)
MAC_pepper(hash)
```

Anforderungen:
- Pepper geheim halten, z. B. in HSM oder getrennt von der DB.
- Salt + Hash allein reichen nach DB-Diebstahl nicht für vollständige Offline-Prüfung.
- Pepper-Rotation ist schwierig: Wenn Pepper ersetzt wird, müssen Nutzer sich erneut anmelden bzw. Hashes neu berechnet werden.

### Rundenzahl / Work Factor

Passwort-Hashing wird absichtlich teuer gemacht:

```text
Hash^N(salt || password)
```

Eine höhere Rundenzahl erhöht die Kosten für legitime Anmeldung und Angreifer gleichermaßen, ist aber gegen Offline-Brute-Force sinnvoll.

## 3.5 Geeignete Passwort-Hashverfahren

| Verfahren | Einordnung |
|---|---|
| **MD5 / SHA-1 allein** | Nicht geeignet: zu schnell, oft ohne Salt verwendet. |
| **MD5crypt** | Laut Foliensatz mittlerweile unsicher/veraltet. |
| **SHA-crypt** | Besser als MD5crypt, variabler Salt und Work Factor. |
| **scrypt** | Memory-hard; erschwert GPU/ASIC-Massenangriffe. |
| **Argon2** | Moderne Empfehlung, Sieger der Password Hashing Competition; memory-hard. |

### Warum memory-hard?

GPU, FPGA und ASIC können sehr viele einfache Hashes parallel berechnen. Arbeitsspeicher ist schwieriger/teurer massiv zu skalieren als reine Rechenoperationen. Memory-hard Verfahren verlangen viel Speicher und bestrafen Berechnung mit zu wenig Speicher.

**Prüfungsfalle:** Ein schneller kryptographischer Hash ist gut für Integrität, aber schlecht für Passwortspeicherung.

## 3.6 Passwortlänge und Passphrasen

Lange zufällig gewählte Passphrasen sind besser merkbar und schwerer zu erraten als kurze, künstlich „komplexe“ Passwörter.

Beispielidee:

```text
mehrere zufällige Wörter > kurzes Wort mit Symbolen/Ziffern
```

Entscheidend ist die Entropie: Wörter müssen zufällig gewählt werden, nicht ein bekannter Satz oder Songtext.

## 3.7 Prüfung auf bekannte Passwort-Leaks

Das in den Folien gezeigte Prinzip nutzt k-Anonymity:
1. Passwort wird lokal im Browser gehasht.
2. Nur die ersten fünf Hex-Zeichen des SHA-1-Hashes werden an den Dienst gesendet.
3. Dienst liefert Hash-Suffixe passender bekannter kompromittierter Passwörter.
4. Browser prüft lokal, ob der vollständige Hash enthalten ist.

Der Server erhält nicht den vollständigen Passwort-Hash; bei einem nicht kompromittierten Passwort sind die übermittelten 20 Bit nicht ausreichend, um das Passwort direkt zu brechen.

---

# 4. Biometrische Verfahren

## 4.1 Grundidee

Biometrie misst körperliche oder verhaltensbezogene Merkmale und vergleicht sie mit einem Referenzmuster.

Beispiele:
- Iris
- Retina
- Fingerabdruck
- Gesicht
- Handvenen
- Stimme
- Unterschrift
- Tippverhalten
- Gangerkennung

Biometrie wird oft gleichzeitig zur Identifizierung und Authentifizierung verwendet.

**Wichtig:** Anders als bei Passwörtern ist keine 100%-Übereinstimmung möglich. Es wird mit Ähnlichkeitswert und Schwellwert gearbeitet.

## 4.2 Statische und dynamische Biometrie

| Kategorie | Beispiele |
|---|---|
| **Physiologisch / statisch** | Fingerabdruck, Gesicht, Iris, Retina, Handgeometrie |
| **Verhaltenstypisch / dynamisch** | Stimme, Unterschrift, Tippverhalten, Gangerkennung |

## 4.3 Anforderungen an biometrische Merkmale

| Kriterium | Bedeutung |
|---|---|
| **Universalität** | Merkmal kommt bei möglichst vielen Menschen vor. |
| **Eindeutigkeit** | Merkmal ist bei unterschiedlichen Personen ausreichend verschieden. |
| **Konstanz** | Merkmal bleibt über Zeit ausreichend stabil. |
| **Messbarkeit** | Sensor kann Merkmal zuverlässig und bezahlbar erfassen. |
| **Anwenderfreundlichkeit** | Verfahren ist komfortabel, akzeptiert und vertraut. |
| **Fälschungssicherheit** | Merkmal ist schwer zu imitieren oder zu stehlen. |

## 4.4 Funktionsprinzip

```text
Sensor
-> Rohdaten
-> Vorverarbeitung
-> Merkmalsextraktion
-> Merkmalsvektor
-> Matching mit Referenzmuster
-> Score
-> Vergleich mit Threshold
-> Accept / Reject
```

Vorverarbeitung kann beinhalten:
- Ausrichten
- Zuschneiden
- Rauschunterdrückung

Beim Fingerabdruck können z. B. **Minutien** extrahiert werden: Rillenenden, Verzweigungen, Positionen und Orientierungen.

## 4.5 Verifikation vs. Identifikation

| Verfahren | Vergleich | Frage |
|---|---|---|
| **Verifikation (one-to-one)** | vorgelegtes Merkmal gegen behauptete Identität | „Bin ich die Person, die ich behaupte zu sein?“ |
| **Identifikation (one-to-many)** | Merkmal gegen viele gespeicherte Referenzmuster | „Wer bin ich?“ |

## 4.6 Fehlerraten

| Kennzahl | Bedeutung |
|---|---|
| **FAR – False Acceptance Rate** | Wie oft wird ein Fremder fälschlich akzeptiert? |
| **FRR – False Rejection Rate** | Wie oft wird ein legitimer Nutzer fälschlich abgelehnt? |

Der Schwellwert ist ein Trade-off:

- niedriger Threshold → mehr Akzeptanzen → **FAR steigt**, FRR sinkt.
- hoher Threshold → mehr Ablehnungen → **FRR steigt**, FAR sinkt.

## 4.7 Risiken biometrischer Verfahren

Biometrische Daten können kopiert, aus Fotos rekonstruiert oder von Gegenständen übernommen werden, z. B.:

- Fingerabdruck aus Foto oder von einem Glas,
- Iris/Gesicht aus Bildmaterial,
- Angriff auf Gesichtserkennung mit Kamera,
- Nachbildung von Venenmustern,
- Offenlegung von Merkmalen in sozialen Medien.

**Zentraler Unterschied zu Passwörtern:** Biometrische Merkmale können bei Kompromittierung nicht einfach geändert werden.

---

# 5. One-Time Passwords (OTP)

## 5.1 Idee

Ein **OTP** ist nur einmal bzw. in einem kurzen Zeitraum gültig.

Einsatz:
- wenn Passworteingabe riskant ist, etwa bei Abhören oder Phishing,
- als zusätzlicher Faktor bei 2FA, insbesondere bei separatem Token/Smartphone.

## 5.2 Sicherheitseigenschaften

OTP schützt gegen:
- passives Mithören,
- Replay-Angriffe.

OTP schützt **nicht automatisch** gegen:
- Man-in-the-Middle,
- Echtzeit-Phishing,
- einen nicht authentisierten Server.

Bei zähler- oder zeitbasierten OTPs müssen Server und Nutzergerät synchron bleiben. Abgebrochene Vorgänge müssen behandelt werden; Nachsynchronisation kann nötig sein.

## 5.3 RSA SecurID

Eigenschaften:
- Token und Server haben synchronisierte Uhren.
- Token erzeugt alle 30 oder 60 Sekunden neuen Code.
- Code basiert auf:
  - Zeit,
  - gerätespezifischem Secret/Seed,
  - optional PIN.
- Wird typischerweise zusätzlich zu Benutzername + Passwort verwendet.

Risiko: Werden Seeds und Seriennummern kompromittiert, kann der Sicherheitswert vieler Tokens gleichzeitig gefährdet sein.

## 5.4 HOTP

**HOTP – HMAC-Based One-Time Password** ist zählerbasiert.

```text
HOTP(K, C) = Truncate(HMAC-SHA-1_K(C))
```

| Symbol | Bedeutung |
|---|---|
| `K` | gemeinsames Geheimnis von User/Token und Server |
| `C` | Zähler |
| `HMAC` | standardisierte MAC-Konstruktion aus Hashfunktion und Schlüssel |
| `Truncate` | Abbildung auf einen meist 6- bis 8-stelligen Dezimalcode |

Problem: Zählerstände können auseinanderlaufen.

## 5.5 TOTP

**TOTP – Time-Based One-Time Password** ist zeitbasiert.

```text
TOTP = HOTP(K, T)
T = (T_current - T_0) / X
```

| Symbol | Bedeutung |
|---|---|
| `T_current` | aktuelle Unix-Zeit |
| `T_0` | Startzeitpunkt, typischerweise Unix Epoch |
| `X` | Zeitfenster, Standard meist 30 Sekunden |

Beispiel: Google Authenticator nutzt TOTP.

---

# 6. Kryptographische Challenge-Response-Verfahren

## 6.1 Prinzip

Der Server stellt eine frische Herausforderung (**Challenge**, oft Zufallszahl). Der Client berechnet eine Antwort (**Response**) mit seinem Geheimnis. Dadurch weist der Client die Kenntnis nach, ohne das Geheimnis direkt zu übertragen.

Frische Challenges verhindern Replay-Angriffe.

## 6.2 Symmetrische Variante
- Client und Server besitzen gemeinsames Geheimnis.
- Client beweist Kenntnis durch Verschlüsselung oder MAC über Challenge.
- Problem: gemeinsames Geheimnis muss vertraulich bei Server und Client gespeichert werden.

Risiken:
- Known-Plaintext-/Wörterbuchangriffe bei schlechten Konstruktionen oder schwachen Geheimnissen.
- Replay, wenn Zufallszahlen wiederholt werden.
- MITM, wenn sich der Server nicht ebenfalls authentisiert.

## 6.3 Asymmetrische Variante
- Server speichert Public Key des Clients.
- Client besitzt Private Key.
- Client signiert Challenge.
- Server prüft Signatur mit Public Key.

Vorteile:
- Server muss keinen geheimen Client-Key speichern.
- Kein Shared Secret zwischen jedem Client und Server.

Anforderung:
- Public Key darf nicht manipuliert werden.
- Alternativ weist Client ein Zertifikat vor.

---

# 7. FIDO, FIDO2 und Passkeys

## 7.1 FIDO-Familie

FIDO umfasst offene Standards für phishing-resistente Authentifizierung.

| Verfahren | Zweck |
|---|---|
| **U2F – Universal 2nd Factor / CTAP1** | Zweiter Faktor zusätzlich zu Benutzername + Passwort, häufig Hardware-Token per USB. |
| **UAF – Universal Authentication Factor** | Passwortlose Authentifizierung; Token wird z. B. mit PIN oder Biometrie geschützt. |
| **FIDO2** | Aktueller Standardverbund aus WebAuthn und CTAP2. |

## 7.2 U2F
- Hardware-Token, meist USB.
- Kein spezieller Treiber notwendig.
- Private Keys sind nicht auslesbar.
- Nutzerpräsenz wird etwa mit Knopfdruck bestätigt.
- Ergänzt üblicherweise Passwort + Benutzername.

## 7.3 FIDO2 und WebAuthn

FIDO2 umfasst:
- **WebAuthn**: W3C-JavaScript-API für Erzeugung und Nutzung von Credentials.
- **CTAP2**: Kommunikation zwischen Client und Authenticator, z. B. über USB-HID, BLE oder NFC.

Authenticator-Arten:

| Typ | Beschreibung |
|---|---|
| **Software Authenticator** | Nur softwarebasiert; weniger sicher. |
| **Platform Authenticator** | Im Gerät eingebaut, z. B. TPM oder Secure Element. |
| **Roaming Authenticator** | Externes oder anderes Gerät via USB, Bluetooth Low Energy oder NFC. |

## 7.4 FIDO2-Architektur

| Rolle | Bedeutung |
|---|---|
| **RP – Relying Party** | Dienst/Webserver, bei dem Anmeldung erfolgt. |
| **FIDO Server** | WebAuthn-Serverkomponente/Library beim RP. |
| **Authenticator / Device** | Gerät oder Token, das Schlüssel erzeugt und Signaturen erstellt. |
| **Metadata Service** | Liefert Informationen über zertifizierte Tokens, Herstellerzertifikate und unterstützte Algorithmen. |

## 7.5 Sicherheitseigenschaften von FIDO2
- Challenge-Response zwischen RP und Authenticator.
- Kein Shared Secret wie bei TOTP.
- RSA- oder ECDSA-Signaturen.
- Brute Force auf den Schlüssel praktisch aussichtslos.
- Pro Dienst ein eigenes Schlüsselpaar: **Nicht-Verkettbarkeit** zwischen Diensten.
- Mehrere Schlüssel pro Dienst möglich; Geräte können geteilt werden.
- Keine OTP-Eingabe, daher weniger Phishing-/Abhörfläche.
- Response ist an die konkrete Relying Party gebunden → stark gegen Phishing.
- **Attestation** kann Typ/Echtheit des Authenticators nachweisen.

## 7.6 Passkeys

Passkeys bauen auf WebAuthn/FIDO auf.

Sie adressieren Grenzen klassischer U2F-Tokens:
- keine zwingenden separaten Hardware-Tokens,
- Nutzung eines Smartphones als Roaming Authenticator,
- Bluetooth kann räumliche Nähe zum PC nachweisen,
- Multi-device Credentials können über OS/Cloud synchronisiert werden, z. B. iCloud Keychain oder Google Password Manager.

Vorteil: bessere Usability bei weiterhin phishing-resistenter kryptographischer Authentisierung.  
Trade-off: Synchronisation über Cloud/Plattform wird Teil des Vertrauensmodells.

---

# 8. Single Sign-On (SSO)

## 8.1 Prinzip

Bei **Single Sign-On** authentifiziert sich der Nutzer einmal beim Identity Provider und kann anschließend mehrere Dienste nutzen, ohne für jeden Dienst erneut ein Passwort einzugeben.

Rollen:

| Rolle | Bedeutung |
|---|---|
| **IdP – Identity Provider** | Authentifiziert Nutzer und stellt Identitätsinformationen/Assertions aus. |
| **RP – Relying Party / Service Provider** | Dienst, der auf Authentisierung durch IdP vertraut. |

Vorteile:
- weniger Passwörter,
- zentrale starke Authentisierung,
- besseres Nutzererlebnis,
- zentralisierte Verwaltung und Widerruf.

Risiken:
- IdP wird besonders kritisches Ziel,
- Kompromittierung/Verfügbarkeit des IdP betrifft viele Dienste,
- IdP kann häufig Nutzungsverhalten sehen.

---

# 9. Shibboleth und SAML

## 9.1 Shibboleth

Shibboleth wird häufig in Hochschulföderationen genutzt, etwa für den Zugriff auf Verlagsangebote.

Zusätzlich zur Authentisierung kann Autorisierung über Gruppen-/Attributinformationen erfolgen. Das wird als **AAI – Authentication and Authorization Infrastructure** bezeichnet.

| Rolle | Bedeutung |
|---|---|
| **IdP** | Heimateinrichtung, bei der der Nutzer den Account hat. |
| **RP / Service Provider** | Anbieter der Ressource, z. B. Verlag. |
| **WAYF – Where Are You From** | Dienst mit Liste von IdPs, damit Nutzer zu ihrer Heimateinrichtung umgeleitet werden können. |

Grundidee:

```text
Nutzer -> Service Provider
-> Auswahl/Ermittlung Heimateinrichtung (WAYF)
-> Redirect zum IdP
-> Login beim IdP
-> signierte Assertion zum Service Provider
-> Zugriff je nach Attributen/Berechtigungen
```

## 9.2 SAML

**SAML – Security Assertion Markup Language**, aktuelle Version 2.0.

- XML-basierte Datenstruktur.
- Kann gemäß XML Encryption und XML Signature verschlüsselt und/oder signiert werden.
- Wird bei Web-SSO über Browser ausgetauscht; daher ist kryptographische Absicherung nötig.

Rollen:

| SAML-Begriff | Entsprechung |
|---|---|
| **Asserting Party / SAML Authority** | Identity Provider |
| **Relying Party** | Service Provider |

### SAML Assertions

| Assertionstyp | Aussage |
|---|---|
| **Authentication Assertion** | Wer ist der Nutzer, wann und wie wurde er authentifiziert? |
| **Authorization Assertion** | Darf das Subjekt auf eine Ressource zugreifen? |
| **Attribute Assertion** | Zusätzliche Attribute, z. B. Gruppenzugehörigkeit oder Kreditlimit. |

---

# 10. OpenID und OpenID Connect

## 10.1 Klassisches OpenID

OpenID ist ein dezentrales SSO-Verfahren für das Web.

Ablauf:

1. Nutzer ruft Relying Party auf.
2. RP leitet per `302 Redirect` an IdP/OpenID Provider weiter.
3. Nutzer authentifiziert sich beim IdP und kontrolliert freizugebende Attribute/Ziel.
4. IdP leitet Nutzer mit Angaben zurück zur RP.
5. RP und IdP kommunizieren direkt zur Prüfung der Authentisierungsantwort, z. B. via Diffie-Hellman.

Alle Verbindungen müssen TLS-geschützt sein.

### Bewertung

| Vorteile | Nachteile |
|---|---|
| Nutzer kontrolliert übertragene Attribute | IdP ist attraktives Phishing-Ziel |
| Teilidentitäten möglich | IdP sieht Nutzungsverhalten |
| IdP frei wählbar / selbst betreibbar | RP muss IdP vertrauen oder Identität zusätzlich prüfen |
| IdP kann starke Authentisierung anbieten | Vorteil begrenzt, wenn RP erneut prüfen muss |
| relativ leicht implementierbar | — |

### OpenID-Phishing

Ein Angreifer kann als RP und gefälschter IdP auftreten. Der Fake-IdP imitiert den echten IdP und stiehlt Nutzerpasswörter.

Gegenmaßnahme laut Folien: zertifikatsbasierte Authentisierung am IdP.

## 10.2 OpenID Connect (OIDC)

OIDC basiert auf **OAuth 2.0** und wird heute weit verbreitet verwendet.

### Authorization-Code-Flow

1. Web-App/RP leitet Browser zum OpenID Provider weiter.
2. Anfrage enthält u. a.:
   - `client_id`
   - `redirect_uri`
   - `scope`
   - `response_type=code`
   - `state`
3. Nutzer meldet sich beim OP an.
4. OP leitet Browser mit kurzlebigem **Authorization Code** zur `redirect_uri` der Web-App zurück.
5. Web-App tauscht Code serverseitig über Backchannel gegen Tokens ein.
6. Web-App authentisiert sich dabei am OP z. B. mit `client_id` + `client_secret`; Secret muss vertraulich bleiben.

### Bedeutung von `state`

`state` speichert Zustand der RP und wird vom OP zurückgegeben. Es dient insbesondere dem Schutz gegen CSRF.

---

# 11. OIDC Tokens und JWT

## 11.1 Identity Token

- Beweist gegenüber der Web-App die Identität des Nutzers.
- Kurze Gültigkeit, typischerweise wenige Minuten.
- Format bei OIDC immer **JWT – JSON Web Token**.
- Wichtige Claims:
  - `issuer`
  - `subject`
  - `audience`
  - `expiration time`
  - `iat` (*issued at*)

## 11.2 Access Token

- Dient zum Zugriff auf APIs.
- Meist kurze Gültigkeit.
- Oft in Header:

```http
Authorization: Bearer <access-token>
```

- Format:
  - JWT, oder
  - opaque zufälliger String; dann muss Dienst beim Token-Introspection-Endpoint prüfen.

## 11.3 Refresh Token

- Dient der Erneuerung von Access-/ID-Tokens ohne erneuten Login.
- Deutlich längere Gültigkeit, z. B. Tage oder Wochen.
- Häufig opaque.
- Empfehlung: **Refresh Token Rotation**. Nach Verwendung wird neues Refresh Token ausgegeben; erneute Verwendung des alten Tokens deutet auf Kompromittierung hin.

---

# 12. Vergleich wichtiger Verfahren

| Verfahren | Typ | Hauptvorteil | Hauptschwäche |
|---|---|---|---|
| Passwort | Wissen | Einfach, universell | Phishing, Wiederverwendung, Offline-Cracking |
| OTP/TOTP | Besitz + ggf. Wissen | Einmaligkeit, Replay-Schutz | Echtzeit-Phishing/MITM, Synchronisation |
| Biometrie | Sein | Komfortabel, schwer zu vergessen | FAR/FRR, Fälschbarkeit, nicht widerrufbar |
| Challenge-Response | Sym./asym. Kryptographie | Geheimnis wird nicht direkt übertragen | Schlüsselverwaltung, ggf. MITM ohne gegenseitige Auth. |
| FIDO2/Passkey | Public-Key, Besitz + lokale User Verification | Phishing-resistent, kein Shared Secret, pro Dienst Keys | Geräte-/Recovery-/Plattformabhängigkeit |
| SAML/Shibboleth | Föderiertes SSO | Hochschul-/Unternehmensföderationen, Attribute | IdP als kritischer Vertrauensanker |
| OIDC | Föderiertes SSO/API | Modernes Web-/API-SSO, OAuth-Ökosystem | Token-/Redirect-/Client-Secret-Sicherheit nötig |

---

# 13. Klausur-Checkliste

Du solltest erklären können:

1. Identität, Identifizierung, Authentifizierung, Autorisierung und Zugriffskontrolle unterscheiden.
2. Die drei Faktoren Wissen, Besitz und Biometrie nennen sowie echte MFA begründen.
3. Warum erneute Authentisierung für kritische Aktionen sinnvoll ist.
4. Passwort-Best-Practices aus dem Foliensatz nennen.
5. Online- und Offline-Passwortangriffe abgrenzen.
6. Salt, Pepper und Work Factor erklären.
7. Warum MD5/SHA-1 allein ungeeignet für Passwortspeicherung sind.
8. Warum scrypt/Argon2 memory-hard sind und weshalb das Angreifer bremst.
9. Biometrische Verfahren, FAR, FRR und Threshold-Trade-off erklären.
10. Verifikation und Identifikation in der Biometrie unterscheiden.
11. HOTP und TOTP inklusive Formel/Parameter erklären.
12. Warum OTP Replay verhindert, aber nicht zwingend MITM/Phishing.
13. Symmetrisches und asymmetrisches Challenge-Response vergleichen.
14. U2F, UAF und FIDO2 einordnen.
15. WebAuthn, CTAP2, RP, Authenticator und Attestation erklären.
16. Weshalb FIDO2/Passkeys phishingsicherer als Passwort + OTP sind.
17. SSO, IdP und RP erklären.
18. Shibboleth, WAYF und AAI einordnen.
19. SAML Assertions unterscheiden.
20. Klassisches OpenID und OIDC abgrenzen.
21. OIDC Authorization-Code-Flow erklären.
22. ID Token, Access Token, Refresh Token und Refresh Token Rotation unterscheiden.

---

## Quellenbasis

- Foliensatz **„IT-Sicherheit 3 – Authentifikationsverfahren“**, Prof. Dr. Tobias Straub, DHBW Stuttgart.
- Themen: Credentials/MFA, Passwörter, Biometrie, OTP, Challenge-Response, FIDO2/Passkeys, SSO, Shibboleth, SAML, OpenID und OpenID Connect.
