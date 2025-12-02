# Regel 13: SSO & Authentifizierung

## Zweck
Diese Regel definiert Standards für die Dokumentation von Single Sign-On (SSO) und Authentifizierungskonfigurationen in ITGlue.

## Unterstützte SSO-Provider

### SSO-Provider Matrix
```markdown
## Unterstützte SSO-Provider

| Provider | SAML 2.0 | Konfigurationsanleitung |
|----------|----------|-------------------------|
| Microsoft Azure AD / Entra ID | ✅ | Azure Enterprise Apps |
| Microsoft ADFS | ✅ | ADFS Relying Party Trust |
| Google Workspace | ✅ | Google Admin Console |
| OneLogin | ✅ | OneLogin Admin Portal |
| Okta | ✅ | Okta Admin Dashboard |
| Centrify | ✅ | Centrify Admin Portal |
| LastPass Enterprise | ✅ | LastPass Admin Console |
| Duo Access Gateway | ✅ | Duo Admin Panel |
| Passly | ✅ | Passly Admin Console |
| Andere SAML 2.0 | ✅ | Generische SAML-Konfiguration |
```

## ITGlue SAML-Konfiguration

### SAML-Einstellungen dokumentieren
```markdown
## ITGlue SAML-Konfiguration

### Service Provider (SP) Details
| Eigenschaft | Wert |
|-------------|------|
| Entity ID | https://[subdomain].itglue.com/ |
| ACS URL | https://[subdomain].itglue.com/users/saml/auth |
| Name ID Format | Email |
| Fingerprint Algorithmus | SHA256 |

### Identity Provider (IdP) Details
| Eigenschaft | Wert |
|-------------|------|
| SSO URL | [IdP SSO URL] |
| Entity ID | [IdP Entity ID] |
| Zertifikat | [Zertifikat-Fingerprint] |
| Fingerprint Algorithmus | SHA256 |
```

### Fingerprint-Berechnung
```markdown
## SAML-Zertifikat Fingerprint

### Windows PowerShell
```powershell
$cert = Get-PfxCertificate -FilePath "idp_certificate.cer"
$cert.Thumbprint
```

### OpenSSL (Linux/Mac)
```bash
openssl x509 -in idp_certificate.cer -fingerprint -sha256 -noout
```

### Hinweise
- Doppelpunkte entfernen
- Nur Großbuchstaben verwenden
- SHA256 als Standard verwenden
```

## Azure AD / Entra ID

### Azure AD SSO Dokumentation
```markdown
## Azure AD / Entra ID SSO-Konfiguration

### Azure Enterprise Application
| Eigenschaft | Wert |
|-------------|------|
| App Name | ITGlue SSO |
| App Type | Enterprise Application (Non-Gallery) |
| SAML Mode | SAML-based Sign-on |

### Azure SAML-Einstellungen
| Azure-Feld | Wert |
|------------|------|
| Identifier (Entity ID) | https://[subdomain].itglue.com/ |
| Reply URL (ACS) | https://[subdomain].itglue.com/users/saml/auth |
| Sign on URL | https://[subdomain].itglue.com/ |
| Relay State | (leer) |
| Logout URL | (optional) |

### Attribute & Claims
| Claim | Value |
|-------|-------|
| Unique User Identifier | user.mail |

### User Assignment
- Benutzer/Gruppen der App zuweisen
- "Assignment required" aktivieren
```

## ADFS-Konfiguration

### ADFS SSO Dokumentation
```markdown
## Microsoft ADFS SSO-Konfiguration

### Relying Party Trust
| Eigenschaft | Wert |
|-------------|------|
| Display Name | ITGlue |
| Relying Party Identifier | https://[subdomain].itglue.com/ |
| SAML Endpoint | https://[subdomain].itglue.com/users/saml/auth |
| Binding | POST |

### Claim Rules
| Regel | Typ | Wert |
|-------|-----|------|
| Email | Send LDAP Attributes | E-Mail → Name ID |

### Token-Einstellungen
- Token-Encryption: Deaktiviert
- Token-Signing: SHA-256
```

## Google Workspace

### Google SSO Dokumentation
```markdown
## Google Workspace SSO-Konfiguration

### SAML App Einstellungen
| Eigenschaft | Wert |
|-------------|------|
| App Name | ITGlue |
| ACS URL | https://[subdomain].itglue.com/users/saml/auth |
| Entity ID | https://[subdomain].itglue.com/ |
| Name ID | Basic Information > Primary Email |
| Name ID Format | EMAIL |

### Service Status
- Status: ON für alle / Organisationseinheiten

### Google IdP Details für ITGlue
- SSO URL aus Google Metadata
- Entity ID aus Google Metadata
- Zertifikat-Download
```

## OneLogin

### OneLogin SSO Dokumentation
```markdown
## OneLogin SSO-Konfiguration

### App Configuration
| Eigenschaft | Wert |
|-------------|------|
| App Name | ITGlue |
| SAML Signature Algorithm | SHA-256 |
| SAML nameID format | Email |

### ITGlue Subdomain
- Subdomain in App-Parameter eingeben

### User Assignment
- Rollen oder Benutzer zuweisen
```

## Okta

### Okta SSO Dokumentation
```markdown
## Okta SSO-Konfiguration

### SAML Settings
| Eigenschaft | Wert |
|-------------|------|
| Single Sign On URL | https://[subdomain].itglue.com/users/saml/auth |
| Audience URI | https://[subdomain].itglue.com/ |
| Name ID Format | EmailAddress |
| Application username | Email |

### Attribute Statements
| Name | Value |
|------|-------|
| email | user.email |

### Group Assignment
- Gruppen für Zugriff zuweisen
```

## Just-in-Time Provisioning

### JIT-Provisioning Dokumentation
```markdown
## Just-in-Time (JIT) Provisioning

### Beschreibung
Automatische Benutzererstellung bei erstem SSO-Login.

### Voraussetzungen
- SSO konfiguriert und aktiv
- JIT in ITGlue aktiviert
- Email-Attribut im SAML-Token

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| JIT Provisioning | Aktiviert |
| Default Role | [Standard-Rolle] |
| Default Group | [Standard-Gruppe] |

### Verhalten
- Neuer Benutzer wird bei SSO-Login erstellt
- Email wird aus SAML-Assertion übernommen
- Standard-Rolle wird zugewiesen
- Benutzer erhält sofort Zugang
```

## SSO-Only Mode

### SSO-Only Dokumentation
```markdown
## SSO-Only Mode (Passwort-Login deaktivieren)

### Beschreibung
Erzwingt ausschließlich SSO-Login, deaktiviert Passwort-Authentifizierung.

### Voraussetzungen
- SSO vollständig getestet
- Alle Benutzer SSO-fähig
- Recovery-Admin identifiziert

### Aktivierung
1. Account > Authentication Settings
2. "Enforce SSO" aktivieren
3. Bestätigen

### Notfall-Recovery
- Account Owner behält Passwort-Login
- Kein Lockout bei IdP-Ausfall
```

## Multi-Faktor-Authentifizierung

### MFA-Dokumentation
```markdown
## Multi-Faktor-Authentifizierung (MFA)

### Unterstützte Methoden
| Methode | Provider |
|---------|----------|
| TOTP | Google Authenticator, Authy, etc. |
| Push | Duo Security |
| Hardware | YubiKey (über TOTP) |

### Erzwingung
| Einstellung | Beschreibung |
|-------------|--------------|
| Optional | Benutzer kann MFA aktivieren |
| Required | MFA wird bei Login erzwungen |

### Konfiguration
- Admin > Settings > Security
- MFA-Richtlinie festlegen
- Grace Period für Einrichtung
```

## SSO Troubleshooting

### SSO-Fehler dokumentieren
```markdown
## SSO Troubleshooting

### Häufige Fehler
| Fehler | Ursache | Lösung |
|--------|---------|--------|
| Invalid Fingerprint | Zertifikat-Mismatch | Fingerprint neu berechnen |
| User Not Found | JIT deaktiviert | JIT aktivieren oder User anlegen |
| Invalid ACS URL | URL-Mismatch | ACS URL prüfen |
| Clock Skew | Zeit-Differenz | NTP synchronisieren |

### Debug-Checkliste
- [ ] Subdomain korrekt?
- [ ] Entity ID korrekt?
- [ ] ACS URL korrekt?
- [ ] Fingerprint aktuell?
- [ ] Name ID Format = Email?
- [ ] Benutzer zugewiesen?
```

## Checkliste SSO-Konfiguration

### Vorbereitung
- [ ] IdP-Admin-Zugang vorhanden
- [ ] ITGlue-Admin-Zugang vorhanden
- [ ] Subdomain identifiziert

### IdP-Konfiguration
- [ ] SAML-App erstellt
- [ ] ACS URL eingetragen
- [ ] Entity ID eingetragen
- [ ] Name ID = Email konfiguriert
- [ ] Benutzer zugewiesen

### ITGlue-Konfiguration
- [ ] IdP SSO URL eingetragen
- [ ] IdP Entity ID eingetragen
- [ ] Fingerprint berechnet und eingetragen
- [ ] Fingerprint-Algorithmus = SHA256

### Test
- [ ] SSO-Login getestet
- [ ] Neuer Benutzer über JIT getestet (falls aktiv)
- [ ] Logout/Re-Login getestet

### Produktiv
- [ ] SSO-Only Mode aktiviert (optional)
- [ ] MFA-Richtlinie definiert
- [ ] Recovery-Prozess dokumentiert
