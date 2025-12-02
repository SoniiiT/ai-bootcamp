# Regel 09: Browser Extension und Passwort-Features

## Zweck
Diese Regel definiert Dokumentationsstandards für die ITGlue Browser Extension, Autofill, OTP und SafeShare-Funktionen.

## Browser Extension Dokumentation

### Extension-Übersicht
```markdown
## ITGlue Browser Extension

### Unterstützte Browser
| Browser | Version | Manifest |
|---------|---------|----------|
| Chrome | Aktuell | V3 |
| Firefox | Aktuell | V3 |
| Edge | Via Chrome Web Store | V3 |

### Funktionen
- Passwort-Suche
- Autofill für Anmeldedaten
- OTP-Generierung
- Quick-Search Hotkey
- Offline-Modus (optional)
```

### Extension-Einstellungen
```markdown
## Extension-Konfiguration

### Allgemeine Einstellungen
| Einstellung | Wert | Beschreibung |
|-------------|------|--------------|
| Subdomain | [subdomain] | ITGlue Account URL |
| Auto-logout | [Minuten] | Inaktivitäts-Timeout |
| Quick-Search Hotkey | [Tastenkombination] | Schnellsuche öffnen |

### Sicherheitseinstellungen
- Biometrische Authentifizierung: Ja/Nein
- Passwort-Caching: [Dauer]
- Automatisches Sperren: [Minuten]
```

## Autofill-Dokumentation

### Autofill-Setup
```markdown
## Autofill-Konfiguration

### Voraussetzungen
- Browser Extension installiert
- Benutzer in ITGlue eingeloggt
- Passwort mit URL gespeichert

### Funktionsweise
1. Website mit gespeichertem Passwort öffnen
2. Extension erkennt URL automatisch
3. Anmeldedaten werden vorgeschlagen
4. Mit Klick oder Hotkey einfügen

### Best Practices
- URL exakt wie in Passwort gespeichert
- Mehrere URLs pro Passwort möglich
- Wildcard-Matching für Subdomains
```

### URL-Matching dokumentieren
```markdown
## URL-Matching für Autofill

### Matching-Regeln
| URL-Typ | Beispiel | Matching |
|---------|----------|----------|
| Exakt | https://portal.example.com | Nur diese URL |
| Domain | example.com | Alle Subdomains |
| Pfad | example.com/admin | Nur mit Pfad |

### Tipps
- HTTPS bevorzugen
- Port mit angeben wenn nicht Standard
- Mehrere URLs für verschiedene Zugriffspunkte
```

## OTP (One-Time Password)

### OTP-Dokumentation
```markdown
## OTP-Konfiguration

### OTP Secret Key speichern
| Feld | Beschreibung |
|------|--------------|
| OTP Secret Key | Base32-codierter Schlüssel |
| Issuer | Name des Dienstes |
| Account | Benutzername/Email |

### OTP-Nutzung
1. Passwort in Extension öffnen
2. OTP-Code wird automatisch generiert
3. Code ist 30 Sekunden gültig
4. Automatisches Kopieren verfügbar

### Export-Hinweis
- OTP Secret Key wird in CSV-Exports einbezogen
- Import von OTP wird NICHT unterstützt
```

### OTP in Listenansicht
```markdown
## OTP Schnellzugriff

### Listenansicht-Feature
- OTP-Code direkt in Passwort-Liste kopieren
- Kein Öffnen des Passwort-Details erforderlich
- Zeigt verbleibende Gültigkeitsdauer
```

## Password SafeShare

### SafeShare-Dokumentation
```markdown
## Password SafeShare

### Funktionsbeschreibung
Passwörter sicher mit externen Personen teilen, die KEIN ITGlue-Konto haben.

### Voraussetzungen
- SafeShare auf Account-Ebene aktiviert
- SafeShare für einzelne Passwörter aktiviert
- Gültige Email-Adresse des Empfängers

### Einschränkungen
- Vaulted Passwords können NICHT geteilt werden
- Ablaufdatum erforderlich
- Maximale Zugriffsanzahl konfigurierbar

### SafeShare-Konfiguration
| Einstellung | Wert | Beschreibung |
|-------------|------|--------------|
| Ablauf | [Stunden/Tage] | Automatische Deaktivierung |
| Max. Zugriffe | [Anzahl] | Limit für Aufrufe |
| Benachrichtigung | Ja/Nein | Email bei Zugriff |
```

### SafeShare-Dokumentationsvorlage
```markdown
## SafeShare-Protokoll: [Passwort-Name]

### Geteilte Informationen
| Datum | Empfänger | Ablauf | Status |
|-------|-----------|--------|--------|
| [Datum] | [Email] | [Datum] | Aktiv/Abgelaufen |

### Zugriffsprotokoll
| Zeitpunkt | IP-Adresse | Status |
|-----------|------------|--------|
| [Datum/Zeit] | [IP] | Erfolgreich |
```

## Passwort-Kategorien

### Kategorien dokumentieren
```markdown
## Passwort-Kategorisierung

### Standard-Kategorien
| Kategorie | Verwendung |
|-----------|------------|
| Credentials | Allgemeine Anmeldedaten |
| Domain Admin | Domain-Administrator |
| Server | Server-Zugänge |
| Network | Netzwerkgeräte |
| Cloud | Cloud-Dienste |
| Vendor | Lieferanten-Portale |

### Benutzerdefinierte Kategorien
| Kategorie | Beschreibung | Erstellt am |
|-----------|--------------|-------------|
| [Name] | [Beschreibung] | [Datum] |

### Kategorie-Richtlinien
- Konsistente Benennung verwenden
- Nicht zu viele Kategorien erstellen
- Regelmäßige Bereinigung durchführen
```

## Offline-Modus

### Offline-Extension dokumentieren
```markdown
## Offline-Modus Konfiguration

### Aktivierung
- Admin > Settings > Offline Mode
- "Enable Offline Mode Extension" aktivieren

### Sicherheitseinstellungen
| Einstellung | Wert | Beschreibung |
|-------------|------|--------------|
| Auto-Wipe | [Tage] | Daten nach X Tagen offline löschen |
| Warnung vor Wipe | [Tage] | Email-Warnung senden |
| Inaktivitäts-Logout | [Minuten] | Automatische Abmeldung |

### SSO im Offline-Modus
- SSO-Benutzer können Offline-Modus nutzen
- Separate Authentifizierung erforderlich
- Konfiguration unter Admin > Settings > Offline Mode
```

## Mobile App

### Mobile-Funktionen dokumentieren
```markdown
## ITGlue Mobile App

### Funktionen
- Passwort-Zugriff
- OTP-Generierung
- Passwort-Generator
- Biometrische Authentifizierung
- QR/Barcode-Scanning

### Plattformen
| Plattform | Version |
|-----------|---------|
| iOS | [Version] |
| Android | [Version] |

### Sicherheit
- Biometrie: Touch ID / Face ID / Fingerprint
- PIN-Fallback verfügbar
- Daten werden verschlüsselt gespeichert
```

## Checkliste Browser Extension

- [ ] Extension installiert und aktuell
- [ ] Subdomain konfiguriert
- [ ] Hotkeys eingerichtet
- [ ] Autofill getestet
- [ ] OTP-Funktionalität geprüft
- [ ] Offline-Modus aktiviert (falls benötigt)
- [ ] Mobile App eingerichtet (optional)
- [ ] Biometrische Authentifizierung konfiguriert
