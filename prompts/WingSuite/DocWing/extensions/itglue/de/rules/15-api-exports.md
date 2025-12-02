# Regel 15: API-Nutzung & Exporte

## Zweck
Diese Regel definiert Standards für die Dokumentation der ITGlue API-Nutzung, Datenexporte und Backup-Strategien.

## API-Grundlagen

### API-Übersicht dokumentieren
```markdown
## ITGlue API: Übersicht

### Basis-Informationen
| Eigenschaft | Wert |
|-------------|------|
| Base URL | https://api.itglue.com |
| EU Base URL | https://api.eu.itglue.com |
| AU Base URL | https://api.au.itglue.com |
| API Version | v1 |
| Format | JSON |
| Authentifizierung | API Key (Header) |

### Rate Limits
| Limit | Wert |
|-------|------|
| Requests pro Minute | [Limit] |
| Bulk Requests | [Limit] |
```

### API-Key Dokumentation
```markdown
## API-Key Management

### Key-Erstellung
| Eigenschaft | Wert |
|-------------|------|
| Key Name | [Beschreibender Name] |
| Erstellt | [Datum] |
| Erstellt von | [Benutzer] |
| Berechtigungen | [Read/Write] |
| Verwendungszweck | [Zweck] |

### Sicherheit
- Keys niemals in Code committen
- Umgebungsvariablen verwenden
- Regelmäßige Rotation
- Nicht mehr benötigte Keys löschen

### Header-Format
```
x-api-key: [API_KEY]
Content-Type: application/vnd.api+json
```
```

## Pagination

### Pagination dokumentieren
```markdown
## API-Pagination

### Standard-Pagination
| Parameter | Beschreibung | Standard |
|-----------|--------------|----------|
| page[size] | Einträge pro Seite | 50 |
| page[number] | Seitennummer | 1 |

### Response-Metadaten
```json
{
  "meta": {
    "current-page": 1,
    "next-page": 2,
    "prev-page": null,
    "total-pages": 10,
    "total-count": 500
  }
}
```

### Alle Seiten abrufen
```powershell
$page = 1
$allData = @()
do {
    $response = Invoke-RestMethod -Uri "$baseUrl?page[number]=$page"
    $allData += $response.data
    $page++
} while ($response.meta.'next-page')
```
```

## Filtering & Sorting

### Filter-Dokumentation
```markdown
## API-Filtering

### Standard-Filter
| Parameter | Beispiel | Beschreibung |
|-----------|----------|--------------|
| filter[id] | 123 | Nach ID filtern |
| filter[name] | Server01 | Nach Name filtern |
| filter[organization_id] | 100 | Nach Organisation |
| filter[updated_at] | 2024-01-01 | Nach Datum |

### Datum-Filter (RFC3339)
```
filter[updated_at]=2024-01-01T00:00:00Z
filter[created_at]=gt:2024-01-01T00:00:00Z
```

### Sortierung
| Parameter | Beschreibung |
|-----------|--------------|
| sort=name | Aufsteigend nach Name |
| sort=-name | Absteigend nach Name |
| sort=created_at | Nach Erstelldatum |
```

### Filter-Kombination
```markdown
## Komplexe Filter

### Beispiel: Aktuelle Konfigurationen
```
/configurations
  ?filter[organization_id]=100
  &filter[configuration_status_id]=active
  &sort=-updated_at
  &page[size]=100
```

### Beispiel: Passwörter mit Ablaufdatum
```
/passwords
  ?filter[password_expiring]=true
  &filter[organization_id]=100
  &sort=password_expires_at
```
```

## PowerShell-Backup-Skripte

### Backup-Skript Dokumentation
```markdown
## ITGlue Backup mit PowerShell

### Voraussetzungen
- PowerShell 5.1 oder höher
- API-Key mit Leserechten
- Ausreichend Speicherplatz

### Basis-Skript
```powershell
# Konfiguration
$apiKey = $env:ITGLUE_API_KEY
$baseUrl = "https://api.itglue.com"
$headers = @{
    "x-api-key" = $apiKey
    "Content-Type" = "application/vnd.api+json"
}

# Organisationen exportieren
$orgs = Invoke-RestMethod -Uri "$baseUrl/organizations" -Headers $headers
$orgs.data | ConvertTo-Json -Depth 10 | Out-File "organizations.json"
```

### Vollständiges Backup-Skript
[Link zu vollständigem Skript]
```

### Export-Typen
```markdown
## Exportierbare Ressourcen

### Ressourcen-Matrix
| Ressource | Endpunkt | Export möglich |
|-----------|----------|----------------|
| Organizations | /organizations | ✅ |
| Configurations | /configurations | ✅ |
| Passwords | /passwords | ✅ |
| Documents | /documents | ✅ |
| Flexible Assets | /flexible_assets | ✅ |
| Contacts | /contacts | ✅ |
| Locations | /locations | ✅ |
| Domains | /domains | ✅ |

### Exportformat
- JSON (API-native)
- CSV (nach Konvertierung)
```

## CSV-Export (UI)

### UI-Export dokumentieren
```markdown
## ITGlue CSV-Export (Benutzeroberfläche)

### Verfügbare Exporte
| Bereich | Export-Option |
|---------|---------------|
| Konfigurationen | Filter → Export CSV |
| Kontakte | Filter → Export CSV |
| Passwörter | Begrenzt (Sicherheit) |
| Dokumente | Nicht direkt |

### Export-Schritte
1. Zur Asset-Liste navigieren
2. Filter anwenden
3. "Export" klicken
4. Format wählen (CSV)
5. Download

### Einschränkungen
- Passwort-Werte nicht exportierbar (UI)
- Große Datenmengen: API bevorzugen
- Embedded Passwords: Nur über API
```

## Backup-Strategie

### Backup-Konzept dokumentieren
```markdown
## ITGlue Backup-Strategie

### Backup-Frequenz
| Datentyp | Frequenz | Aufbewahrung |
|----------|----------|--------------|
| Organisationen | Wöchentlich | 90 Tage |
| Konfigurationen | Täglich | 30 Tage |
| Passwörter | Täglich | 90 Tage |
| Dokumente | Wöchentlich | 90 Tage |
| Flexible Assets | Täglich | 30 Tage |

### Backup-Speicherort
| Speicherort | Verschlüsselung | Zugriff |
|-------------|-----------------|---------|
| Azure Blob | AES-256 | RBAC |
| AWS S3 | Server-side | IAM |
| On-Premises | BitLocker | AD |

### Backup-Validierung
- Monatliche Restore-Tests
- Integritätsprüfung
- Stichproben-Validierung
```

## API-Dokumentation Templates

### Endpunkt-Dokumentation
```markdown
## API-Endpunkt: [Endpunkt-Name]

### Übersicht
| Eigenschaft | Wert |
|-------------|------|
| Methode | GET/POST/PATCH/DELETE |
| Endpunkt | /[resource] |
| Authentifizierung | API Key |

### Request
```http
GET /organizations HTTP/1.1
Host: api.itglue.com
x-api-key: [API_KEY]
Content-Type: application/vnd.api+json
```

### Response
```json
{
  "data": [...],
  "meta": {...},
  "links": {...}
}
```

### Fehler-Codes
| Code | Bedeutung |
|------|-----------|
| 200 | Erfolgreich |
| 401 | Nicht autorisiert |
| 403 | Verboten |
| 404 | Nicht gefunden |
| 429 | Rate Limit |
```

## Automatisierte Exporte

### Automatisierung dokumentieren
```markdown
## Automatisierte ITGlue-Exporte

### Scheduled Tasks (Windows)
```powershell
# Task erstellen
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" `
    -Argument "-File C:\Scripts\ITGlue-Backup.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At 2am
Register-ScheduledTask -TaskName "ITGlue Backup" `
    -Action $action -Trigger $trigger
```

### Cron (Linux)
```bash
# Täglich um 2 Uhr
0 2 * * * /usr/local/bin/itglue-backup.sh
```

### Azure Automation
- Runbook erstellen
- Schedule definieren
- Key in Key Vault speichern
```

## Error Handling

### Fehlerbehandlung dokumentieren
```markdown
## API-Fehlerbehandlung

### Retry-Logik
```powershell
$maxRetries = 3
$retryCount = 0
do {
    try {
        $response = Invoke-RestMethod -Uri $url -Headers $headers
        break
    }
    catch {
        $retryCount++
        if ($retryCount -ge $maxRetries) { throw }
        Start-Sleep -Seconds (2 * $retryCount)
    }
} while ($retryCount -lt $maxRetries)
```

### Rate Limit Handling
- 429-Response erkennen
- Retry-After Header beachten
- Exponential Backoff

### Logging
- Alle API-Calls loggen
- Fehler dokumentieren
- Performance-Metriken sammeln
```

## Best Practices API

### API Best Practices
```markdown
## ITGlue API: Best Practices

### Performance
- Pagination nutzen (nicht alle Daten auf einmal)
- Filter verwenden um Datenmenge zu reduzieren
- Parallele Requests vermeiden (Rate Limits)

### Sicherheit
- API-Keys nicht hardcoden
- Secrets in Vault speichern
- Least Privilege für API-Keys

### Wartbarkeit
- Versionierung der Skripte
- Dokumentation aktuell halten
- Änderungen protokollieren

### Monitoring
- API-Verfügbarkeit überwachen
- Backup-Erfolg prüfen
- Fehlgeschlagene Calls alertieren
```

## Checkliste API & Exporte

### API-Setup
- [ ] API-Key erstellt
- [ ] Key sicher gespeichert
- [ ] Basis-URL korrekt
- [ ] Test-Request erfolgreich

### Backup-Setup
- [ ] Backup-Skript erstellt
- [ ] Automatisierung konfiguriert
- [ ] Speicherort definiert
- [ ] Verschlüsselung aktiv

### Monitoring
- [ ] Backup-Erfolg wird geprüft
- [ ] Fehler werden gemeldet
- [ ] Restore-Tests geplant
