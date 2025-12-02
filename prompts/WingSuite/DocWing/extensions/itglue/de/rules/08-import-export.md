# Regel 08: Import und Export

## Zweck
Diese Regel definiert Standards für die Dokumentation von Datenimport, Export und Backup-Verfahren in ITGlue.

## CSV-Import

### Import-Voraussetzungen dokumentieren
```markdown
## CSV-Import Vorbereitung

### Datei-Anforderungen
- Format: CSV (Comma Separated Values)
- Encoding: UTF-8 empfohlen
- Trennzeichen: Komma (,) oder Semikolon (;)
- Header-Zeile: Erforderlich

### Regionale Einstellungen
| Region | Dezimaltrennzeichen | Tausendertrennzeichen | Listentrennzeichen |
|--------|--------------------|-----------------------|-------------------|
| US/UK | . | , | , |
| DE/EU | , | . | ; |

### Vor dem Import
- [ ] Backup der bestehenden Daten
- [ ] CSV-Datei validiert
- [ ] Spaltenzuordnung geprüft
- [ ] Testdatensatz importiert
```

### Import-Typen dokumentieren
```markdown
## Verfügbare Import-Typen

### Organisationen
| ITGlue Feld | CSV-Spalte | Erforderlich |
|-------------|-----------|--------------|
| name | Organization Name | Ja |
| short_name | Short Name | Nein |
| description | Description | Nein |
| organization_status | Status | Nein |

### Kontakte
| ITGlue Feld | CSV-Spalte | Erforderlich |
|-------------|-----------|--------------|
| first_name | First Name | Ja |
| last_name | Last Name | Ja |
| primary_email | Email | Nein |
| title | Job Title | Nein |

### Passwörter
| ITGlue Feld | CSV-Spalte | Erforderlich |
|-------------|-----------|--------------|
| name | Password Name | Ja |
| username | Username | Nein |
| password | Password | Ja |
| url | URL | Nein |
| notes | Notes | Nein |

### Flexible Assets
| ITGlue Feld | CSV-Spalte | Erforderlich |
|-------------|-----------|--------------|
| organization_name | Organization | Ja |
| [Feldname] | [Entsprechende Spalte] | Abhängig |
```

## Flexible Asset Templates importieren

### Template-Import dokumentieren
```markdown
## Flexible Asset Template Import

### Verfügbare Templates
| Template Name | Kategorie | Felder |
|---------------|-----------|--------|
| Application | Apps & Services | [Anzahl] |
| LAN | Infrastructure | [Anzahl] |
| Licensing | Administration | [Anzahl] |

### Import-Prozess
1. Admin > Flexible Asset Types
2. Import > Flexible Asset Template
3. Template auswählen
4. Felder anpassen (optional)
5. Import bestätigen

### Nach dem Import
- Sidebar-Position anpassen
- Berechtigungen konfigurieren
- Testdatensatz erstellen
```

## Daten-Export

### Export-Dokumentation
```markdown
## Daten-Export: [Datum]

### Geplanter Export
| Einstellung | Wert |
|-------------|------|
| Zeitplan | Täglich/Wöchentlich/Monatlich |
| Exporttyp | Vollständig/Inkrementell |
| Ziel | Email/Download |
| Empfänger | [Email-Adressen] |

### Exportierte Daten
- [ ] Organisationen
- [ ] Kontakte
- [ ] Konfigurationen
- [ ] Passwörter
- [ ] Dokumente
- [ ] Flexible Assets
- [ ] Domains
- [ ] SSL-Zertifikate

### Exportformat
| Datentyp | Dateiname | Format |
|----------|-----------|--------|
| Passwords | passwords.csv | CSV |
| Organizations | organizations.csv | CSV |
| Configurations | configurations.csv | CSV |
```

## Backup-Verfahren

### Backup-Dokumentation
```markdown
## ITGlue Backup-Strategie

### Automatische Exports
| Frequenz | Inhalt | Aufbewahrung |
|----------|--------|--------------|
| Täglich | Passwörter | 30 Tage |
| Wöchentlich | Vollständig | 12 Wochen |
| Monatlich | Vollständig | 12 Monate |

### API-basiertes Backup (PowerShell)
```powershell
# Beispiel-Skript zur Dokumentation
$ApiKey = Get-Content "path/to/apikey.txt"
$BaseUrl = "https://api.itglue.com"
# Weitere Backup-Logik...
```

### Disaster Recovery
- Recovery-Kontakt: Kaseya Support
- RTO: [Zeit]
- RPO: [Zeit]
- Letzte Testwiederherstellung: [Datum]
```

## Network Detective Import

### Import von Network Detective
```markdown
## Network Detective Import

### Voraussetzungen
- Network Detective Scan abgeschlossen
- Export im ITGlue-Format erstellt
- Zielorganisation in ITGlue vorhanden

### Importierte Daten
| Datentyp | Verfügbar |
|----------|-----------|
| Computer | Ja |
| Netzwerkgeräte | Ja |
| Software | Ja |
| Benutzer | Ja |

### Nachbearbeitung
- [ ] Daten auf Vollständigkeit prüfen
- [ ] Konfigurationstypen zuweisen
- [ ] Flexible Assets erstellen
```

## OTP Secret Key Export

### OTP in Exports
```markdown
## OTP Export-Hinweise

### Export-Verhalten
- OTP Secret Key wird in passwords.csv exportiert
- Spalte: 'otp_secret_key'
- Nur bei manuellem/geplantem Export

### Import-Einschränkung
- OTP Secret Key Import wird NICHT unterstützt
- OTP muss manuell neu eingerichtet werden
```

## Import-Fehlerbehandlung

### Fehler dokumentieren
```markdown
## Import-Fehlerprotokoll

### Häufige Fehler
| Fehler | Ursache | Lösung |
|--------|---------|--------|
| Encoding-Fehler | Falsche Zeichenkodierung | UTF-8 verwenden |
| Feld nicht gefunden | Spaltenname falsch | Mapping prüfen |
| Duplikat | Eintrag existiert bereits | Überspringen/Aktualisieren |
| Organisation nicht gefunden | Zielorg fehlt | Erst Organisation erstellen |

### Fehlerprotokoll-Format
| Zeile | Feld | Fehler | Wert | Status |
|-------|------|--------|------|--------|
| 5 | email | Ungültiges Format | "test@" | Übersprungen |
```

## Checkliste Import/Export

### Vor dem Import
- [ ] Backup erstellt
- [ ] CSV-Datei validiert
- [ ] Encoding geprüft (UTF-8)
- [ ] Spaltenzuordnung dokumentiert
- [ ] Testlauf durchgeführt

### Nach dem Import
- [ ] Datenzählung verifiziert
- [ ] Stichproben geprüft
- [ ] Fehlerprotokoll ausgewertet
- [ ] Berechtigungen gesetzt

### Für Exports
- [ ] Exportumfang definiert
- [ ] Zeitplan eingerichtet
- [ ] Empfänger konfiguriert
- [ ] Speicherort gesichert
- [ ] Aufbewahrungsrichtlinie dokumentiert
