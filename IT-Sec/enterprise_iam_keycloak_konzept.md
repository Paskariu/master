# Projektkonzept: Enterprise Identity and Access Management mit Keycloak

**Seminar:** IT-Sicherheit  
**Format:** 25-minütige Präsentation mit Beispielprojekt + Seminararbeit im IEEE-Manuscript-Template, ca. 6 Seiten  
**Arbeitstitel:** *Modernes Enterprise Identity and Access Management mit Keycloak, LDAP, Single Sign-On und TOTP-Mehrfaktorauthentifizierung*

---

## 1. Zielsetzung des Projekts

Ziel des Projekts ist es, zu zeigen, wie eine Organisation Identitäten, Authentifizierung und Berechtigungen zentral verwalten kann, statt jede Anwendung mit eigener Benutzerverwaltung, eigener Passwortdatenbank und eigenen Rollenmodellen auszustatten.

Das Projekt verbindet drei Ebenen:

1. **Konzeptionelle IAM-Ebene**  
   Begriffe, Rollenmodelle, Benutzerlebenszyklus, Berechtigungen, Single Sign-On und föderierte Identitäten. Als Grundlagenquellen eignen sich besonders Tsolkas/Schmidt: [*Rollen und Berechtigungskonzepte*](https://link.springer.com/book/10.1007/978-3-658-17987-8), von Faber: [„Identitäts- und Zugriffsmanagement (IAM)”](https://link.springer.com/chapter/10.1007/978-3-658-33431-4_3) sowie Schwartz/Machulak: [*Securing the Perimeter*](https://link.springer.com/book/10.1007/978-1-4842-2601-8).

2. **Technische Protokollebene**  
   OAuth 2.0, OpenID Connect, LDAP, optional SAML und TOTP. Relevante Primärquellen sind [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/), [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html), [RFC 4511: LDAPv3](https://datatracker.ietf.org/doc/rfc4511/), [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238) und der [SAML 2.0 Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html).

3. **Praktische Umsetzungsebene**  
   Keycloak wird als Identity Provider und Identity Broker genutzt. OpenLDAP dient als zentrales Benutzerverzeichnis. Zwei Demo-Webanwendungen zeigen SSO, rollenbasierten Zugriff und TOTP-2FA. Die produktnahe Grundlage bilden der [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html) und [Getting Started with Keycloak on Docker](https://www.keycloak.org/getting-started/getting-started-docker).

### Leitfrage

> Wie kann ein modernes Enterprise Identity and Access Management mit Keycloak, LDAP, Single Sign-On und TOTP-Mehrfaktorauthentifizierung umgesetzt werden, und welche Sicherheitsvorteile sowie Restrisiken ergeben sich daraus?

### Geplanter Beitrag der Arbeit

Die Arbeit soll nicht nur erklären, was IAM ist, sondern anhand einer kleinen, nachvollziehbaren Laborumgebung demonstrieren:

- zentrale Benutzerverwaltung über LDAP,
- Authentifizierung über Keycloak,
- Single Sign-On für mehrere Anwendungen,
- Rollen- und Gruppenabbildung,
- Mehrfaktorauthentifizierung mit TOTP,
- Sicherheitsbewertung typischer IAM-Risiken.

---

## 2. Fachlicher Zuschnitt und Abgrenzung

### Im Fokus

- Enterprise IAM als Sicherheits- und Organisationskonzept,
- Keycloak als zentrale IAM-Komponente,
- LDAP/OpenLDAP als Verzeichnisdienst, stellvertretend für LDAP/Active Directory-Strukturen,
- OpenID Connect als modernes Web-SSO-Protokoll,
- TOTP als zweiter Faktor,
- rollenbasierte Zugriffskontrolle in zwei Demo-Anwendungen,
- sicherheitliche Bewertung von SSO, Tokens, MFA und zentralem Identity Provider.

### Bewusste Abgrenzung

Nicht vollständig behandelt werden:

- produktiver Betrieb eines Active Directory Forests,
- vollständige SAML-Konfiguration mit externem Enterprise-IdP,
- vollständige Zero-Trust-Architektur,
- produktionsreife Hochverfügbarkeit von Keycloak,
- vollständiges Identity Governance and Administration, z. B. Rezertifizierung, SoD-Prüfungen und komplexe Joiner-Mover-Leaver-Prozesse.

Diese Abgrenzung ist notwendig, weil Präsentation und Seminararbeit kompakt bleiben müssen. Zero Trust kann aber als sicherheitlicher Ausblick verwendet werden, gestützt auf [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final).

---

# 3. Struktur der Präsentation, Gesamtumfang 25 Minuten

## Präsentationsziel

Die Präsentation soll dem Publikum in 25 Minuten erklären:

1. welches Problem IAM in Organisationen löst,
2. welche Komponenten ein modernes IAM-System enthält,
3. wie Keycloak, LDAP, OIDC und TOTP zusammenspielen,
4. wie die Demo-Architektur funktioniert,
5. welche Sicherheitsvorteile und Restrisiken entstehen.

## Grober Zeitplan

| Abschnitt | Zeit | Inhalt | Quellenbasis |
|---|---:|---|---|
| 1. Einstieg und Motivation | 2 min | Problem vieler dezentraler Accounts, Schatten-IT, Passwort-Wiederverwendung, manuelle Rechtevergabe | Tsolkas/Schmidt; von Faber |
| 2. IAM-Grundlagen | 4 min | Identität, Authentifizierung, Autorisierung, Rollen, Gruppen, Benutzerlebenszyklus | Tsolkas/Schmidt; Schwartz/Machulak |
| 3. Architektur moderner IAM-Systeme | 4 min | Verzeichnisdienst, Identity Provider, Service Provider/Relying Party, Federation, SSO | Keycloak Doku; OIDC Core; LDAP RFC |
| 4. Protokolle und Sicherheitsmechanismen | 4 min | OAuth 2.0, OIDC, Tokens, Claims, TOTP, optional SAML-Abgrenzung | RFC 6749; OIDC Core; RFC 6238; SAML Overview |
| 5. Live-Demo | 8 min | Login, SSO, Rollenprüfung, Admin-Zugriff, TOTP, optional externer IdP | Keycloak Doku; Docker-Guide |
| 6. Bewertung und Fazit | 3 min | Vorteile, Risiken, Best Practices, Ausblick auf Zero Trust und phishing-resistente MFA | NIST SP 800-63B-4; NIST SP 800-207; Fett et al. |

---

## 3.1 Detaillierte Folienstruktur

### Folie 1: Titel und Leitfrage

**Titel:** Modernes Enterprise Identity and Access Management mit Keycloak  
**Kernaussage:** Eine zentrale IAM-Plattform kann Identitäten, Anmeldung und Rechtevergabe konsolidieren.

**Unterpunkte:**

- Ausgangslage: mehrere Anwendungen, mehrere Benutzerverwaltungen.
- Ziel: zentrale Identität, ein Login, einheitliche Rollen.
- Leitfrage der Arbeit.

**Quellen:**

- [Tsolkas/Schmidt, Rollen und Berechtigungskonzepte](https://link.springer.com/book/10.1007/978-3-658-17987-8)
- [von Faber, Identitäts- und Zugriffsmanagement](https://link.springer.com/chapter/10.1007/978-3-658-33431-4_3)

---

### Folie 2: Problemstellung in Organisationen

**Kernaussage:** Dezentrale Benutzerverwaltungen erzeugen Sicherheits- und Verwaltungsprobleme.

**Unterpunkte:**

- Neue Mitarbeitende müssen in mehreren Systemen angelegt werden.
- Beim Rollenwechsel bleiben alte Rechte oft bestehen.
- Beim Austritt müssen Konten in vielen Anwendungen deaktiviert werden.
- Anwendungen implementieren eigene Passwortlogik und eigene Rollenmodelle.
- Auditierbarkeit und Nachvollziehbarkeit sind erschwert.

**Quellen:**

- [Tsolkas/Schmidt, Rollen und Berechtigungskonzepte](https://link.springer.com/book/10.1007/978-3-658-17987-8)
- [von Faber, IAM-Kapitel](https://link.springer.com/chapter/10.1007/978-3-658-33431-4_3)

---

### Folie 3: Zentrale Begriffe

**Kernaussage:** IAM trennt Identität, Authentifizierung und Autorisierung.

**Unterpunkte:**

- **Identität:** digitale Repräsentation einer Person oder technischen Entität.
- **Authentifizierung:** Nachweis, dass die Entität wirklich die behauptete Identität besitzt.
- **Autorisierung:** Entscheidung, worauf die authentifizierte Identität zugreifen darf.
- **Rollen/Gruppen:** Abstraktion von Einzelrechten.
- **Provisioning:** Erzeugen, Ändern und Entfernen von Konten und Berechtigungen.

**Quellen:**

- [Tsolkas/Schmidt, Rollen und Berechtigungskonzepte](https://link.springer.com/book/10.1007/978-3-658-17987-8)
- [Schwartz/Machulak, Securing the Perimeter](https://link.springer.com/book/10.1007/978-1-4842-2601-8)

---

### Folie 4: Zielarchitektur eines Enterprise-IAM

**Kernaussage:** Moderne IAM-Architekturen bündeln Identitätsdaten, Authentifizierung und Zugriffskontrolle in klar getrennten Komponenten.

```text
Benutzer
   |
   v
Keycloak als Identity Provider / Identity Broker
   |
   +--> LDAP / Active Directory als Benutzer- und Gruppenquelle
   |
   +--> Web-App 1: Mitarbeiterportal
   |
   +--> Web-App 2: Admin-Dashboard
   |
   +--> optional: externer Identity Provider
```

**Unterpunkte:**

- LDAP/AD speichert Benutzer und Gruppen.
- Keycloak übernimmt Login, Sessions, Token-Ausgabe und Identity Brokering.
- Anwendungen vertrauen Keycloak und müssen Passwörter nicht selbst verwalten.
- Rollen werden als Claims im Token an Anwendungen übergeben.

**Quellen:**

- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)
- [RFC 4511: LDAP](https://datatracker.ietf.org/doc/rfc4511/)
- [Smirnov, Building Modern Active Directory](https://link.springer.com/book/10.1007/979-8-8688-0941-5)

---

### Folie 5: LDAP und Active Directory im IAM-Kontext

**Kernaussage:** LDAP/AD dient als zentrale Quelle für Benutzer-, Gruppen- und Organisationsinformationen.

**Unterpunkte:**

- LDAP ist ein Protokoll für den Zugriff auf Verzeichnisdienste.
- Active Directory nutzt LDAP-kompatible Mechanismen, erweitert diese aber um Microsoft-spezifische Funktionen.
- In der Demo wird OpenLDAP verwendet, um das Prinzip ohne Windows-Domänenumgebung zu zeigen.
- Keycloak kann Benutzer aus LDAP/AD föderieren und Gruppen/Rollen abbilden.

**Quellen:**

- [RFC 4511: LDAPv3](https://datatracker.ietf.org/doc/rfc4511/)
- [Smirnov, Building Modern Active Directory](https://link.springer.com/book/10.1007/979-8-8688-0941-5)
- [Keycloak Server Administration Guide: User Federation](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

### Folie 6: Single Sign-On mit OpenID Connect

**Kernaussage:** OpenID Connect baut auf OAuth 2.0 auf und ergänzt eine Identitätsschicht für Login und Benutzerinformationen.

**Unterpunkte:**

- OAuth 2.0 beschreibt Autorisierung und Token-Flows.
- OpenID Connect ergänzt ID Token, UserInfo Endpoint und standardisierte Claims.
- Keycloak agiert als OpenID Provider.
- Die Demo-Anwendungen sind OIDC-Clients bzw. Relying Parties.
- Nach erfolgreichem Login erstellt Keycloak eine Session; weitere Anwendungen können diese für SSO nutzen.

**Quellen:**

- [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [Fett/Küsters/Schmitz, The Web SSO Standard OpenID Connect](https://research.lancaster-university.uk/en/publications/the-web-sso-standard-openid-connect-in-depth-formal-security-anal/)

---

### Folie 7: Token, Claims und Rollen

**Kernaussage:** Anwendungen entscheiden nicht mehr anhand eigener Passwortdatenbanken, sondern anhand vertrauenswürdiger Tokens und Claims.

**Unterpunkte:**

- **ID Token:** enthält Informationen über die angemeldete Identität.
- **Access Token:** wird für den Zugriff auf Ressourcen/APIs verwendet.
- **Claims:** strukturierte Aussagen über Benutzer, z. B. Benutzername, Gruppen oder Rollen.
- **Rollenprüfung:** Anwendung erlaubt Admin-Funktionen nur, wenn das Token eine Admin-Rolle enthält.
- **Sicherheitsaspekt:** Tokens müssen geschützt, validiert und zeitlich begrenzt sein.

**Quellen:**

- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/)
- [RFC 9700: Best Current Practice for OAuth 2.0 Security](https://datatracker.ietf.org/doc/rfc9700/)

---

### Folie 8: Mehrfaktorauthentifizierung mit TOTP

**Kernaussage:** TOTP ergänzt Passwort-Login um einen zeitbasierten zweiten Faktor, ist aber nicht phishing-resistent.

**Unterpunkte:**

- TOTP erzeugt Einmalpasswörter auf Basis eines gemeinsamen Geheimnisses und der aktuellen Zeit.
- Typischerweise 6-stelliger Code, periodisch neu berechnet.
- Praktische Nutzung über Authenticator-App.
- Keycloak kann OTP/TOTP als Required Action oder über Authentication Flows erzwingen.
- Einschränkung: TOTP schützt gegen Passwortdiebstahl, aber nicht zuverlässig gegen Echtzeit-Phishing.

**Quellen:**

- [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238)
- [NIST SP 800-63B-4: Authentication and Authenticator Management](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)
- [Keycloak Server Administration Guide: OTP Policies / Authentication Flows](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

### Folie 9: Demo-Überblick

**Kernaussage:** Die Demo zeigt eine kleine Unternehmensumgebung mit zentralem Identity Provider, LDAP-Benutzerquelle, zwei Anwendungen, SSO, Rollen und TOTP.

**Demo-Szenario:**

- Benutzerin `alice` ist normale Mitarbeiterin.
- Benutzer `bob` ist Administrator.
- Beide Benutzer liegen in OpenLDAP.
- Keycloak importiert oder föderiert die Benutzer.
- Zwei Anwendungen nutzen Keycloak per OIDC:
  - `employee-portal`
  - `admin-dashboard`
- `alice` erhält Zugriff auf das Mitarbeiterportal, aber nicht auf Admin-Funktionen.
- `bob` erhält Zugriff auf beide Anwendungen.
- Nach Aktivierung von TOTP wird beim Login ein zweiter Faktor verlangt.

**Quellen:**

- [Keycloak Getting Started with Docker](https://www.keycloak.org/getting-started/getting-started-docker)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

### Folie 10: Live-Demo, Schrittfolge

**Kernaussage:** Die Demo macht IAM durch konkrete Nutzer-, Rollen- und Login-Flows sichtbar.

**Ablauf:**

1. Start der Docker-Umgebung.
2. Öffnen von Keycloak Admin Console.
3. Anzeigen des Realms `dhbw-company`.
4. Anzeigen der LDAP-Föderation.
5. Login als `alice` im Mitarbeiterportal.
6. Öffnen des Admin-Dashboards im selben Browser: SSO greift, aber Admin-Zugriff wird verweigert.
7. Login als `bob`: Admin-Zugriff wird erlaubt.
8. Aktivierung von TOTP für einen Benutzer.
9. Logout und erneuter Login mit Passwort + OTP-Code.
10. Anzeigen der Token-Claims, insbesondere Benutzername, Gruppen und Rollen.

**Quellen:**

- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238)

---

### Folie 11: Sicherheitsbewertung

**Kernaussage:** IAM erhöht Sicherheit und Verwaltbarkeit, erzeugt aber auch neue zentrale Risiken.

**Vorteile:**

- zentrale Benutzer- und Rechteverwaltung,
- weniger Passwortdatenbanken in Fachanwendungen,
- SSO mit kontrollierter Session-Verwaltung,
- konsistente MFA-Vorgaben,
- bessere Auditierbarkeit,
- zentrale Deaktivierung von Konten.

**Restrisiken:**

- Keycloak als zentrales Ziel für Angreifer,
- Fehlkonfiguration von Clients und Redirect URIs,
- zu breite Rollen oder falsch gemappte Gruppen,
- Token-Diebstahl,
- TOTP ist nicht phishing-resistent,
- Verfügbarkeit des Identity Providers wird kritisch.

**Quellen:**

- [Fett/Küsters/Schmitz, A Comprehensive Formal Security Analysis of OAuth 2.0](https://research.lancaster-university.uk/en/publications/a-comprehensive-formal-security-analysis-of-oauth-20-2/)
- [Fett/Küsters/Schmitz, The Web SSO Standard OpenID Connect](https://research.lancaster-university.uk/en/publications/the-web-sso-standard-openid-connect-in-depth-formal-security-anal/)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)
- [RFC 9700: OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/rfc9700/)

---

### Folie 12: Fazit und Ausblick

**Kernaussage:** Keycloak eignet sich gut, um Enterprise-IAM-Konzepte praktisch zu demonstrieren; produktiv sind zusätzliche Härtungsmaßnahmen erforderlich.

**Unterpunkte:**

- Zentrales IAM reduziert Komplexität in Anwendungen.
- OIDC ermöglicht modernes Web-SSO.
- LDAP/AD bleibt als Unternehmensverzeichnis relevant.
- TOTP ist eine praktikable MFA-Demo, aber langfristig sind phishing-resistente Verfahren wie FIDO2/WebAuthn stärker.
- Zero Trust erweitert IAM um kontinuierliche Prüfung, Kontext und Least Privilege.

**Quellen:**

- [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

# 4. Genaues Konzept des Demo-Projekts

## 4.1 Ziel der Demo

Die Demo soll nicht nur zeigen, dass Keycloak funktioniert, sondern mehrere IAM-Konzepte in einer konsistenten Mini-Unternehmensumgebung verbinden:

- zentrales Benutzerverzeichnis,
- Identity Provider,
- OIDC-basierter Login,
- SSO über mehrere Anwendungen,
- rollenbasierter Zugriff,
- TOTP als zweiter Faktor,
- kurze Sicherheitsanalyse anhand sichtbarer Token-Claims.

---

## 4.2 Vorgeschlagene Demo-Architektur

```text
+-------------------+        +-------------------+
|                   |        |                   |
|   OpenLDAP        |<------>|   Keycloak        |
|   Benutzer        |        |   Identity        |
|   Gruppen         |        |   Provider        |
|                   |        |   Broker          |
+-------------------+        +---------+---------+
                                      |
                         OIDC Auth Code Flow
                                      |
              +-----------------------+-----------------------+
              |                                               |
              v                                               v
+-----------------------------+              +-----------------------------+
| employee-portal             |              | admin-dashboard             |
| normale Mitarbeiter-App     |              | Admin-Anwendung             |
| Rolle: employee             |              | Rolle: admin erforderlich   |
+-----------------------------+              +-----------------------------+
```

Optional kann ein zweiter Keycloak-Realm oder eine zweite Keycloak-Instanz als externer OIDC-Identity-Provider eingebunden werden, um Identity Brokering zu zeigen. Diese Erweiterung ist nur sinnvoll, wenn die Basisdemo stabil läuft.

**Quellen:**

- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 4511: LDAP](https://datatracker.ietf.org/doc/rfc4511/)

---

## 4.3 Komponenten

| Komponente | Funktion | Technologievorschlag | Begründung | Quelle |
|---|---|---|---|---|
| Identity Provider | Login, Sessions, Token-Ausgabe, Rollen, MFA | Keycloak | Open Source, unterstützt OIDC, SAML, LDAP/AD, Identity Brokering und OTP | [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html) |
| Verzeichnisdienst | Benutzer- und Gruppenquelle | OpenLDAP | Leichtgewichtig in Docker; zeigt LDAP-Prinzip ohne Windows-Domäne | [RFC 4511](https://datatracker.ietf.org/doc/rfc4511/) |
| Mitarbeiterportal | OIDC-Client für normale Nutzer | Python Flask/FastAPI oder Node.js Express | Kleine Web-App, Login per Keycloak, Anzeige der Claims | [OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html) |
| Admin-Dashboard | OIDC-Client mit Rollenprüfung | Python Flask/FastAPI oder Node.js Express | Demonstriert Autorisierung anhand von Rollenclaims | [RFC 6749](https://datatracker.ietf.org/doc/rfc6749/) |
| TOTP | Zweiter Faktor | Keycloak OTP + Authenticator App | Einfache, sichtbare MFA-Demo | [RFC 6238](https://datatracker.ietf.org/doc/html/rfc6238) |
| Datenbank | Persistenz für Keycloak | PostgreSQL, optional H2 nur für schnelle Demo | PostgreSQL ist näher an realer Umgebung; H2 genügt für Minimaldemo | [Keycloak Docker Guide](https://www.keycloak.org/getting-started/getting-started-docker) |

---

## 4.4 Docker-Compose-Aufbau

### Minimalvariante

Für eine robuste Vorführung reicht eine Minimalvariante:

```text
docker-compose.yml
├── keycloak
├── openldap
├── ldap-admin-ui optional
├── employee-portal
└── admin-dashboard
```

### Empfohlene Variante

Für eine sauberere Demo:

```text
demo-project/
├── docker-compose.yml
├── keycloak/
│   ├── realm-export.json
│   └── themes/ optional
├── ldap/
│   ├── bootstrap.ldif
│   └── users.ldif
├── employee-portal/
│   ├── Dockerfile
│   ├── app.py oder server.js
│   └── requirements.txt oder package.json
├── admin-dashboard/
│   ├── Dockerfile
│   ├── app.py oder server.js
│   └── requirements.txt oder package.json
└── README.md
```

**Quellen:**

- [Keycloak Getting Started with Docker](https://www.keycloak.org/getting-started/getting-started-docker)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

## 4.5 Konkrete Demo-Konfiguration

### Realm

```text
Realm: dhbw-company
```

### LDAP-Struktur

```text
dc=dhbw,dc=local
├── ou=people
│   ├── uid=alice
│   └── uid=bob
└── ou=groups
    ├── cn=employees
    └── cn=it-admins
```

### Benutzer

| Benutzer | Gruppe | Rolle in Keycloak | Demo-Zweck |
|---|---|---|---|
| `alice` | `employees` | `employee` | normale Mitarbeiterin |
| `bob` | `employees`, `it-admins` | `employee`, `admin` | Administrator |

### Keycloak-Rollen

```text
employee
admin
```

### Clients

| Client | Typ | Redirect URI | Zweck |
|---|---|---|---|
| `employee-portal` | OIDC Client | `http://localhost:8001/callback` | Mitarbeiterportal |
| `admin-dashboard` | OIDC Client | `http://localhost:8002/callback` | Admin-App |

Für Webanwendungen sollte der Authorization Code Flow genutzt werden. Für produktive Public Clients ist PKCE relevant; für serverseitige vertrauliche Clients wird zusätzlich ein Client Secret verwendet. Die sicherheitliche Einordnung sollte mit [RFC 6749](https://datatracker.ietf.org/doc/rfc6749/), [RFC 7636: PKCE](https://datatracker.ietf.org/doc/html/rfc7636) und [RFC 9700: OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/rfc9700/) begründet werden.

---

## 4.6 Ablauf der Live-Demo

### Schritt 1: Umgebung starten

```bash
docker compose up -d
```

**Zu zeigen:**

- alle Container laufen,
- Keycloak Admin Console erreichbar,
- OpenLDAP läuft,
- beide Demo-Apps erreichbar.

---

### Schritt 2: Benutzerquelle zeigen

**Zu zeigen:**

- LDAP-Baum mit `alice` und `bob`,
- Gruppen `employees` und `it-admins`,
- Keycloak User Federation.

**Erklärung:**

OpenLDAP übernimmt in der Demo die Rolle des zentralen Benutzerverzeichnisses. In realen Organisationen wäre diese Komponente häufig Microsoft Active Directory oder ein anderer Enterprise Directory Service.

**Quelle:** [RFC 4511: LDAP](https://datatracker.ietf.org/doc/rfc4511/)

---

### Schritt 3: Login im Mitarbeiterportal

**Ablauf:**

1. Browser öffnet `employee-portal`.
2. App erkennt fehlende Session.
3. Redirect zu Keycloak.
4. Login als `alice`.
5. Redirect zurück zur App.
6. App zeigt Benutzername und Claims.

**Erklärung:**

Dies demonstriert OpenID Connect als Login-Schicht über OAuth 2.0. Die Anwendung authentifiziert den Benutzer nicht selbst, sondern vertraut dem von Keycloak ausgestellten Token.

**Quellen:**

- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/)

---

### Schritt 4: SSO demonstrieren

**Ablauf:**

1. Im gleichen Browser `admin-dashboard` öffnen.
2. Kein erneuter Passwort-Login nötig.
3. Keycloak erkennt bestehende SSO-Session.
4. Benutzer wird zurück zur Admin-App geleitet.

**Erklärung:**

Single Sign-On entsteht, weil der Benutzer bereits eine aktive Session beim Identity Provider besitzt. Die zweite Anwendung nutzt denselben Identity Provider und muss den Benutzer nicht erneut authentifizieren.

**Quelle:** [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)

---

### Schritt 5: Rollenbasierte Autorisierung zeigen

**Ablauf mit `alice`:**

- `alice` kann das Mitarbeiterportal öffnen.
- `alice` erhält im Admin-Dashboard eine Fehlermeldung: `403 Forbidden` oder „Admin role required“.

**Ablauf mit `bob`:**

- Logout oder neuer Browser.
- Login als `bob`.
- `bob` kann das Admin-Dashboard öffnen.

**Erklärung:**

Die Anwendung prüft nicht das Passwort, sondern Rollenclaims im Token. Die Rolle `admin` entscheidet über den Zugriff auf Admin-Funktionen.

**Quellen:**

- [Tsolkas/Schmidt, Rollen und Berechtigungskonzepte](https://link.springer.com/book/10.1007/978-3-658-17987-8)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)

---

### Schritt 6: TOTP aktivieren

**Ablauf:**

1. In Keycloak für `bob` Required Action `Configure OTP` setzen.
2. Login als `bob`.
3. QR-Code mit Authenticator-App scannen.
4. OTP-Code eingeben.
5. Logout und erneuter Login.
6. Login verlangt Passwort + TOTP-Code.

**Erklärung:**

TOTP basiert auf einem geteilten Geheimnis und einer zeitbasierten Berechnung. Der zweite Faktor reduziert das Risiko kompromittierter Passwörter, bleibt aber anfällig für Echtzeit-Phishing.

**Quellen:**

- [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)

---

### Schritt 7: Token-Claims zeigen

**Zu zeigen:**

- Username,
- E-Mail,
- Gruppen,
- Rollen,
- Token-Ablaufzeit,
- Issuer,
- Audience.

**Erklärung:**

Dieser Schritt verbindet Theorie und Demo: Die Studierenden sehen, welche Informationen der Identity Provider an die Anwendung übergibt und wie daraus Zugriffskontrolle entsteht.

**Quellen:**

- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/)

---

## 4.7 Optionaler Demo-Ausbau: Identity Brokering

Falls noch Zeit und Stabilität vorhanden sind, kann ein zweiter Identity Provider simuliert werden:

```text
External IdP Realm: partner-idp
Company Realm: dhbw-company
```

Der Realm `dhbw-company` bindet `partner-idp` als externen OpenID-Connect-Provider ein. Dadurch kann gezeigt werden, wie Keycloak als Identity Broker zwischen Anwendungen und externen Identity Providern agiert.

**Nutzen für die Präsentation:**

- erfüllt den Stichpunkt „Anbindung verschiedener Identity Provider via Keycloak“,
- zeigt Federation/Brokering anschaulich,
- bleibt lokal demonstrierbar, ohne echte externe Cloudkonten.

**Quelle:** [Keycloak Server Administration Guide: Identity Brokering](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

## 4.8 Sicherheitsanforderungen an die Demo

Die Demo ist eine Laborumgebung. Trotzdem sollten folgende Punkte erwähnt werden, damit nicht der Eindruck einer produktiven Standardkonfiguration entsteht:

| Bereich | Demo-Vereinfachung | Produktive Anforderung | Quelle |
|---|---|---|---|
| Transport | HTTP auf localhost | HTTPS/TLS verpflichtend | [RFC 6749](https://datatracker.ietf.org/doc/rfc6749/) |
| Redirect URIs | lokale Redirects | exakte Redirect-URI-Validierung, keine Wildcards | [RFC 9700](https://datatracker.ietf.org/doc/rfc9700/) |
| Token | Anzeige zur Erklärung | Tokens nicht im Frontend offenlegen oder loggen | [OIDC Core](https://openid.net/specs/openid-connect-core-1_0.html) |
| MFA | TOTP | besser phishing-resistente Verfahren wie FIDO2/WebAuthn prüfen | [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final) |
| IdP | einzelne Keycloak-Instanz | HA, Backups, Monitoring, sichere Admin-Zugänge | [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html) |
| Rollen | zwei Rollen | Least Privilege, regelmäßige Rezertifizierung | [Tsolkas/Schmidt](https://link.springer.com/book/10.1007/978-3-658-17987-8) |

---

# 5. Aufbau der Seminararbeit, ca. 6 Seiten

Die Seminararbeit sollte nicht als reine Dokumentation der Demo geschrieben werden. Besser ist eine wissenschaftlich-technische Struktur: Problemstellung, Grundlagen, Architektur, Protokolle, Demo, Sicherheitsbewertung.

## Vorgeschlagene Gliederung im IEEE-Stil

### Abstract, ca. 100–150 Wörter

Kurzfassung mit Problem, Ansatz, Demo und Ergebnis.

**Inhalt:**

- Problem dezentraler Identitätsverwaltung,
- Umsetzung mit Keycloak, LDAP, OIDC und TOTP,
- Demo mit zwei Anwendungen,
- Ergebnis: IAM verbessert zentrale Kontrolle und Nutzerfreundlichkeit, erzeugt aber neue zentrale Risiken.

---

### I. Einleitung, ca. 0,5 Seiten

**Ziel:** Problem und Relevanz darstellen.

**Inhalt:**

- Organisationen betreiben viele Anwendungen mit unterschiedlichen Zugriffsanforderungen.
- Dezentrale Benutzerverwaltungen erschweren Sicherheit, Compliance und Administration.
- IAM adressiert diese Probleme durch zentrale Identitäts- und Berechtigungsverwaltung.
- Vorstellung der Forschungsfrage.

**Quellen:**

- [von Faber, IAM](https://link.springer.com/chapter/10.1007/978-3-658-33431-4_3)
- [Tsolkas/Schmidt](https://link.springer.com/book/10.1007/978-3-658-17987-8)

---

### II. Grundlagen von Identity and Access Management, ca. 1 Seite

**Ziel:** Begriffe und konzeptionelle Grundlage schaffen.

**Inhalt:**

- digitale Identität,
- Authentifizierung vs. Autorisierung,
- Benutzer, Gruppen, Rollen, Rechte,
- Role-Based Access Control als praktisches Organisationsmodell,
- Provisioning und Deprovisioning,
- Single Sign-On und Federation.

**Quellen:**

- [Tsolkas/Schmidt](https://link.springer.com/book/10.1007/978-3-658-17987-8)
- [Schwartz/Machulak, Securing the Perimeter](https://link.springer.com/book/10.1007/978-1-4842-2601-8)

---

### III. Architektur eines modernen Enterprise-IAM, ca. 1 Seite

**Ziel:** Systemarchitektur erklären.

**Inhalt:**

- zentrale IAM-Plattform,
- Verzeichnisdienst als Identitätsquelle,
- Keycloak als Identity Provider,
- Anwendungen als OIDC-Clients,
- Mapping von LDAP-Gruppen auf Keycloak-Rollen,
- optionale Anbindung externer Identity Provider.

**Quellen:**

- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)
- [RFC 4511: LDAP](https://datatracker.ietf.org/doc/rfc4511/)
- [Smirnov, Building Modern Active Directory](https://link.springer.com/book/10.1007/979-8-8688-0941-5)

---

### IV. Single Sign-On mit OAuth 2.0 und OpenID Connect, ca. 1–1,2 Seiten

**Ziel:** Protokollmechanik verständlich erklären.

**Inhalt:**

- Rollen in OAuth 2.0: Resource Owner, Client, Authorization Server, Resource Server,
- OpenID Connect als Identitätsschicht über OAuth 2.0,
- Authorization Code Flow,
- ID Token, Access Token, Claims,
- SSO-Sitzung beim Identity Provider,
- kurzer Vergleich zu SAML als klassischem Enterprise-SSO-Standard.

**Quellen:**

- [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)
- [SAML 2.0 Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html)
- [Fett/Küsters/Schmitz, OAuth 2.0 Security Analysis](https://research.lancaster-university.uk/en/publications/a-comprehensive-formal-security-analysis-of-oauth-20-2/)
- [Fett/Küsters/Schmitz, OIDC Security Analysis](https://research.lancaster-university.uk/en/publications/the-web-sso-standard-openid-connect-in-depth-formal-security-anal/)

---

### V. Mehrfaktorauthentifizierung mit TOTP, ca. 0,6–0,8 Seiten

**Ziel:** 2FA technisch und sicherheitlich einordnen.

**Inhalt:**

- Prinzip von TOTP,
- geteiltes Geheimnis und Zeitfenster,
- Einsatz in Authenticator-Apps,
- Umsetzung in Keycloak,
- Vorteile gegenüber reinem Passwortlogin,
- Schwächen gegenüber Phishing und Man-in-the-Middle-Angriffen,
- Ausblick auf stärkere Verfahren wie FIDO2/WebAuthn.

**Quellen:**

- [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

### VI. Demo-Projekt und Implementierung, ca. 0,8–1 Seite

**Ziel:** Die praktische Umsetzung knapp, aber nachvollziehbar beschreiben.

**Inhalt:**

- Docker-basierte Laborumgebung,
- Komponenten: Keycloak, OpenLDAP, zwei Webanwendungen,
- Benutzer- und Rollenmodell,
- OIDC-Client-Konfiguration,
- SSO-Ablauf,
- TOTP-Aktivierung,
- Ergebnis der Rollenprüfung.

**Quellen:**

- [Keycloak Getting Started with Docker](https://www.keycloak.org/getting-started/getting-started-docker)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html)

---

### VII. Sicherheitsbewertung, ca. 0,8 Seiten

**Ziel:** Vorteile und Risiken kritisch diskutieren.

**Inhalt:**

**Vorteile:**

- zentrale Kontrolle,
- konsistente Authentifizierungsrichtlinien,
- weniger Angriffsfläche in Fachanwendungen,
- einfacheres Offboarding,
- bessere Nachvollziehbarkeit.

**Risiken:**

- zentraler Identity Provider als Single Point of Failure und Angriffsziel,
- Fehlkonfiguration von Clients,
- unsichere Redirect URIs,
- Token-Diebstahl,
- Rollenausweitung durch falsches Mapping,
- TOTP nicht phishing-resistent.

**Quellen:**

- [Fett/Küsters/Schmitz, OAuth 2.0 Security Analysis](https://research.lancaster-university.uk/en/publications/a-comprehensive-formal-security-analysis-of-oauth-20-2/)
- [Fett/Küsters/Schmitz, OIDC Security Analysis](https://research.lancaster-university.uk/en/publications/the-web-sso-standard-openid-connect-in-depth-formal-security-anal/)
- [RFC 9700: OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/rfc9700/)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)

---

### VIII. Fazit und Ausblick, ca. 0,4–0,5 Seiten

**Ziel:** Ergebnisse verdichten.

**Inhalt:**

- Keycloak eignet sich als didaktisch und praktisch gut nutzbare IAM-Plattform.
- LDAP/AD bleibt als Identitätsquelle relevant.
- OIDC ist für moderne Webanwendungen zentral.
- TOTP ist als MFA-Einstieg geeignet, aber nicht der höchste Sicherheitsstandard.
- Für produktive Umgebungen sind Härtung, Monitoring, Hochverfügbarkeit und Zero-Trust-Prinzipien notwendig.

**Quellen:**

- [NIST SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
- [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final)
- [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html)

---

# 6. Vorschlag für Arbeitsteilung

| Person | Schwerpunkt | Präsentation | Seminararbeit | Demo-Aufgabe |
|---|---|---|---|---|
| Julia Rick | IAM-Grundlagen und LDAP/AD | Problemstellung, Begriffe, LDAP/AD | Einleitung, Grundlagen, Verzeichnisdienste | OpenLDAP, Benutzer, Gruppen |
| Philipp Ehinger | Keycloak, OIDC, SSO | Architektur, OIDC-Flow, Tokens | Architektur, OIDC/OAuth/SAML | Keycloak Realm, Clients, Rollenmapping |
| Niklas Gotermann | Demo, MFA, Security Bewertung | Live-Demo, TOTP, Sicherheitsbewertung | Demo-Kapitel, MFA, Fazit | Web-Apps, Rollenprüfung, TOTP |

---

# 7. Quellen- und Inhaltsmatrix

Diese Matrix zeigt, welche Quelle für welchen Teil der Präsentation und Seminararbeit verwendet werden sollte.

| Quelle | Typ | Verwendung in Präsentation | Verwendung in Seminararbeit |
|---|---|---|---|
| [Tsolkas/Schmidt: Rollen und Berechtigungskonzepte](https://link.springer.com/book/10.1007/978-3-658-17987-8) | Springer-Buch | IAM-Grundlagen, Rollenmodell | Grundlagen, RBAC, Berechtigungskonzepte |
| [von Faber: Identitäts- und Zugriffsmanagement](https://link.springer.com/chapter/10.1007/978-3-658-33431-4_3) | Springer-Buchkapitel | Motivation und Unternehmensbezug | Einleitung, IAM als Management- und Sicherheitsaufgabe |
| [Schwartz/Machulak: Securing the Perimeter](https://link.springer.com/book/10.1007/978-1-4842-2601-8) | Springer/Apress-Buch | praktische IAM-Architektur | Architektur, Open-Source-IAM, Federation |
| [Smirnov: Building Modern Active Directory](https://link.springer.com/book/10.1007/979-8-8688-0941-5) | Springer/Apress-Buch | AD als Enterprise-Verzeichnis | LDAP/AD-Kontext und moderne AD-Einordnung |
| [RFC 4511: LDAP](https://datatracker.ietf.org/doc/rfc4511/) | Standard | LDAP-Grundlagen | technische Beschreibung von LDAP |
| [RFC 6749: OAuth 2.0](https://datatracker.ietf.org/doc/rfc6749/) | Standard | Authorization Code Flow, Rollenmodell | Protokollgrundlage OAuth 2.0 |
| [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html) | Standard/Spezifikation | SSO, ID Token, Claims | OIDC-Erklärung und Token-Struktur |
| [SAML 2.0 Technical Overview](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html) | Standardnahes OASIS-Dokument | Abgrenzung OIDC/SAML | kurzer Vergleich klassisches Enterprise-SSO |
| [RFC 6238: TOTP](https://datatracker.ietf.org/doc/html/rfc6238) | Standard | TOTP-Demo | technische Erklärung von TOTP |
| [NIST SP 800-63B-4](https://csrc.nist.gov/pubs/sp/800/63/b/4/final) | Richtlinie | Bewertung von MFA | Sicherheitsbewertung, Phishing-Resistenz |
| [NIST SP 800-207](https://csrc.nist.gov/pubs/sp/800/207/final) | Richtlinie | Ausblick Zero Trust | Fazit und architektonische Einordnung |
| [Fett/Küsters/Schmitz: OAuth 2.0 Security Analysis](https://research.lancaster-university.uk/en/publications/a-comprehensive-formal-security-analysis-of-oauth-20-2/) | Peer-reviewed Paper | Risiken von OAuth/SSO | Sicherheitsbewertung OAuth 2.0 |
| [Fett/Küsters/Schmitz: OIDC Security Analysis](https://research.lancaster-university.uk/en/publications/the-web-sso-standard-openid-connect-in-depth-formal-security-anal/) | Peer-reviewed Paper | Risiken von OIDC | Sicherheitsbewertung OIDC |
| [Li/Mitchell: Analysing Google's Implementation of OpenID Connect](https://dl.acm.org/doi/10.1007/978-3-319-40667-1_18) | Peer-reviewed Paper, Springer LNCS | reale Implementierungsrisiken | Diskussion: Standard vs. Implementierung |
| [Keycloak Server Administration Guide](https://www.keycloak.org/docs/latest/server_admin/index.html) | Herstellerdokumentation | Demo-Architektur, User Federation, OTP | Implementierungsteil |
| [Keycloak Docker Guide](https://www.keycloak.org/getting-started/getting-started-docker) | Herstellerdokumentation | Start der Demo | praktischer Aufbau der Laborumgebung |
| [RFC 7636: PKCE](https://datatracker.ietf.org/doc/html/rfc7636) | Standard | optional: sichere Public Clients | Sicherheitsmaßnahmen für OAuth/OIDC |
| [RFC 9700: OAuth 2.0 Security Best Current Practice](https://datatracker.ietf.org/doc/rfc9700/) | Standard/Best Current Practice | Redirect-URI- und Token-Sicherheit | Sicherheitsbewertung und Best Practices |

---

# 8. Literaturverzeichnis, vorläufig im IEEE-nahen Stil

[1] A. Tsolkas and K. Schmidt, *Rollen und Berechtigungskonzepte: Identity- und Access-Management im Unternehmen*. Wiesbaden: Springer Vieweg, 2017. DOI: [10.1007/978-3-658-17987-8](https://doi.org/10.1007/978-3-658-17987-8).  
[2] E. von Faber, “Identitäts- und Zugriffsmanagement (IAM),” in *IT und IT-Sicherheit in Begriffen und Zusammenhängen*, Wiesbaden: Springer Vieweg, 2021, pp. 53–84. DOI: [10.1007/978-3-658-33431-4_3](https://doi.org/10.1007/978-3-658-33431-4_3).  
[3] M. Schwartz and M. Machulak, *Securing the Perimeter: Deploying Identity and Access Management with Free Open Source Software*. Berkeley, CA: Apress, 2018. DOI: [10.1007/978-1-4842-2601-8](https://doi.org/10.1007/978-1-4842-2601-8).  
[4] E. Smirnov, *Building Modern Active Directory: Engineering, Building, and Running Active Directory for the Next 25 Years*. Berkeley, CA: Apress, 2024. DOI: [10.1007/979-8-8688-0941-5](https://doi.org/10.1007/979-8-8688-0941-5).  
[5] J. Sermersheim, Ed., “Lightweight Directory Access Protocol (LDAP): The Protocol,” RFC 4511, IETF, 2006. Available: [https://datatracker.ietf.org/doc/rfc4511/](https://datatracker.ietf.org/doc/rfc4511/)  
[6] D. Hardt, Ed., “The OAuth 2.0 Authorization Framework,” RFC 6749, IETF, 2012. Available: [https://datatracker.ietf.org/doc/rfc6749/](https://datatracker.ietf.org/doc/rfc6749/)  
[7] N. Sakimura, J. Bradley, M. Jones, B. de Medeiros, and C. Mortimore, “OpenID Connect Core 1.0 incorporating errata set 2,” OpenID Foundation, 2023. Available: [https://openid.net/specs/openid-connect-core-1_0.html](https://openid.net/specs/openid-connect-core-1_0.html)  
[8] E. Maler, P. Madsen, and T. Scavo, “SAML 2.0 Technical Overview,” OASIS, 2005. Available: [https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html](https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html)  
[9] D. M'Raihi, S. Machani, M. Pei, and J. Rydell, “TOTP: Time-Based One-Time Password Algorithm,” RFC 6238, IETF, 2011. Available: [https://datatracker.ietf.org/doc/html/rfc6238](https://datatracker.ietf.org/doc/html/rfc6238)  
[10] D. Temoshok, J. Fenton, Y.-Y. Choong, N. Lefkovitz, A. Regenscheid, R. Galluzzo, and J. Richer, *Digital Identity Guidelines: Authentication and Authenticator Management*, NIST SP 800-63B-4, 2025. DOI: [10.6028/NIST.SP.800-63b-4](https://doi.org/10.6028/NIST.SP.800-63b-4).  
[11] S. Rose, O. Borchert, S. Mitchell, and S. Connelly, *Zero Trust Architecture*, NIST SP 800-207, 2020. DOI: [10.6028/NIST.SP.800-207](https://doi.org/10.6028/NIST.SP.800-207).  
[12] D. Fett, R. Küsters, and G. Schmitz, “A Comprehensive Formal Security Analysis of OAuth 2.0,” in *Proceedings of the ACM Conference on Computer and Communications Security*, 2016, pp. 1204–1215. DOI: [10.1145/2976749.2978385](https://doi.org/10.1145/2976749.2978385).  
[13] D. Fett, R. Küsters, and G. Schmitz, “The Web SSO Standard OpenID Connect: In-depth Formal Security Analysis and Security Guidelines,” in *2017 IEEE 30th Computer Security Foundations Symposium (CSF)*, 2017, pp. 189–202. DOI: [10.1109/CSF.2017.20](https://doi.org/10.1109/CSF.2017.20).  
[14] W. Li and C. J. Mitchell, “Analysing the Security of Google’s Implementation of OpenID Connect,” in *Detection of Intrusions and Malware, and Vulnerability Assessment (DIMVA 2016)*, Lecture Notes in Computer Science, Springer, 2016, pp. 357–376. DOI: [10.1007/978-3-319-40667-1_18](https://doi.org/10.1007/978-3-319-40667-1_18).  
[15] Keycloak Project, *Server Administration Guide*. Available: [https://www.keycloak.org/docs/latest/server_admin/index.html](https://www.keycloak.org/docs/latest/server_admin/index.html). Access date should be added in final submission.  
[16] Keycloak Project, *Getting Started with Keycloak on Docker*. Available: [https://www.keycloak.org/getting-started/getting-started-docker](https://www.keycloak.org/getting-started/getting-started-docker). Access date should be added in final submission.  
[17] N. Sakimura, J. Bradley, and N. Agarwal, “Proof Key for Code Exchange by OAuth Public Clients,” RFC 7636, IETF, 2015. Available: [https://datatracker.ietf.org/doc/html/rfc7636](https://datatracker.ietf.org/doc/html/rfc7636)  
[18] T. Lodderstedt, J. Bradley, A. Labunets, and D. Fett, “Best Current Practice for OAuth 2.0 Security,” RFC 9700, IETF, 2025. Available: [https://datatracker.ietf.org/doc/rfc9700/](https://datatracker.ietf.org/doc/rfc9700/)

---

# 9. Bewertung des Konzepts gegenüber der Aufgabenstellung

Das Konzept erfüllt die Anforderungen der Aufgabenstellung aus folgenden Gründen:

1. **Eigene Recherche:**  
   Es werden Springer-Bücher, wissenschaftliche Papers, Standards und Herstellerdokumentation kombiniert.

2. **Technische Details:**  
   OIDC, OAuth 2.0, LDAP, TOTP, Rollenclaims und Keycloak-Architektur werden technisch erklärbar eingebunden.

3. **Praxisnahe Demo:**  
   Die Docker-basierte Demo zeigt nicht nur Login, sondern auch SSO, Rollenprüfung, LDAP-Föderation und TOTP.

4. **Angemessener Umfang:**  
   Die Präsentation bleibt mit 25 Minuten realistisch, wenn die Identity-Brokering-Erweiterung optional bleibt.

5. **Seminararbeit auf 6 Seiten möglich:**  
   Die Arbeit kann konzentriert auf IAM-Grundlagen, Architektur, OIDC/TOTP, Demo und Sicherheitsbewertung geschrieben werden.

---

# 10. Empfohlene Prioritäten für die Umsetzung

## Muss umgesetzt werden

- Keycloak läuft lokal per Docker.
- Realm `dhbw-company` ist eingerichtet.
- Zwei Benutzer und zwei Rollen existieren.
- Eine Web-App nutzt OIDC-Login.
- Zweite Web-App zeigt SSO.
- Admin-Zugriff wird rollenbasiert geprüft.
- TOTP wird für einen Benutzer aktiviert.

## Sollte umgesetzt werden

- OpenLDAP als externe Benutzerquelle.
- Gruppenmapping von LDAP-Gruppen auf Keycloak-Rollen.
- Anzeige der Token-Claims in der Demo-App.
- Dokumentierter Demo-Ablauf mit Screenshots für den Fall, dass die Live-Demo ausfällt.

## Kann umgesetzt werden

- zweiter Keycloak-Realm als externer Identity Provider,
- SAML-Vergleich als kurze Zusatzfolie,
- PKCE-Demonstration,
- FIDO2/WebAuthn als Ausblick.

---

# 11. Risiken und Gegenmaßnahmen für die Vorführung

| Risiko | Auswirkung | Gegenmaßnahme |
|---|---|---|
| Docker-Umgebung startet nicht | Live-Demo fällt aus | Screenshots und kurzer Demo-Clip vorbereiten |
| LDAP-Föderation funktioniert nicht | Kernpunkt Benutzerquelle fehlt | Fallback: Keycloak-interne Benutzer, LDAP nur architektonisch erklären |
| TOTP-App nicht verfügbar | 2FA-Demo fällt aus | Backup-Gerät oder Screenshotfolge vorbereiten |
| Token-Anzeige wirkt zu komplex | Publikum verliert Zusammenhang | Nur 3–4 Claims erklären: `sub`, `preferred_username`, `roles`, `exp` |
| Zeitüberschreitung | Fazit und Bewertung entfallen | Demo strikt auf 8 Minuten begrenzen |
| Identity Brokering zu instabil | unnötige Komplexität | nur optional zeigen oder als Architekturfolie erklären |

---

# 12. Kurzfassung für die Abstimmung mit dem Dozenten

Wir schlagen vor, das Thema **Enterprise Identity and Access Management** anhand einer lokalen Keycloak-Demo umzusetzen. Die Demo besteht aus Keycloak als zentralem Identity Provider, OpenLDAP als Benutzerverzeichnis und zwei Webanwendungen als OIDC-Clients. Gezeigt werden Single Sign-On, rollenbasierte Zugriffskontrolle, LDAP-basierte Benutzerföderation und TOTP-Mehrfaktorauthentifizierung. Optional kann ein zweiter Keycloak-Realm als externer Identity Provider eingebunden werden, um Identity Brokering zu demonstrieren.

Die Seminararbeit behandelt zunächst IAM-Grundlagen, Rollen- und Berechtigungskonzepte, LDAP/AD als Verzeichnisdienst sowie OAuth 2.0 und OpenID Connect als Protokollbasis für SSO. Danach werden TOTP und die Demo-Architektur beschrieben. Abschließend erfolgt eine Sicherheitsbewertung mit Fokus auf zentrale IdP-Risiken, Token-Sicherheit, Client-Fehlkonfigurationen, TOTP-Schwächen und Zero-Trust-Prinzipien.

Die Literaturbasis kombiniert Springer-Quellen zu IAM und Active Directory, Primärstandards zu LDAP, OAuth 2.0, OpenID Connect und TOTP, NIST-Richtlinien zu MFA und Zero Trust sowie wissenschaftliche Arbeiten zur Sicherheit von OAuth 2.0 und OpenID Connect.

