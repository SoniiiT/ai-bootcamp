# Regel 10: PSA-Integration

## Zweck
Diese Regel definiert Standards für die Dokumentation von PSA-Integrationen (Autotask, ConnectWise, Kaseya BMS, Pulseway PSA).

## Allgemeine PSA-Dokumentation

### Integration-Übersicht
```markdown
## PSA-Integration: [PSA-Name]

### Verbindungsinformationen
| Eigenschaft | Wert |
|-------------|------|
| PSA-Typ | Autotask/ConnectWise/BMS/Pulseway |
| API-URL | [URL] |
| Sync-Status | Aktiv/Pausiert |
| Letzte Synchronisation | [Datum/Zeit] |

### Sync-Methode
- [ ] One-Way Sync (PSA → ITGlue)
- [ ] Two-Way Sync (PSA ↔ ITGlue)

### Sync-Einstellungen
| Datentyp | Sync aktiviert |
|----------|---------------|
| Organisationen | Ja/Nein |
| Kontakte | Ja/Nein |
| Konfigurationen | Ja/Nein |
| Standorte | Ja/Nein |
```

## Autotask Integration

### Autotask-Dokumentation
```markdown
## Autotask Integration

### API-Konfiguration
| Einstellung | Wert |
|-------------|------|
| Integration Code | [Code] |
| Username | [API-Benutzer] |
| Secret | [gespeichert als Passwort] |
| Zone | [Zone-URL] |

### Ticket Rules
| Regel-Name | Bedingungen | Aktionen |
|------------|-------------|----------|
| [Name] | [Kriterien] | [IT Glue Dokumente anzeigen] |

### LiveLinks
| Link-Typ | URL-Muster |
|----------|------------|
| IT Glue Org | https://[subdomain].itglue.com/links/autotask/org/ |
| IT Glue Config | https://[subdomain].itglue.com/links/autotask/config/ |
```

### Ticket Insights dokumentieren
```markdown
## Autotask Ticket Insights

### Verfügbare Insights
| Insight | Beschreibung |
|---------|--------------|
| IT Glue Documents | Relevante Dokumente |
| IT Glue Passwords | Relevante Passwörter |
| IT Glue Flexible Assets | Relevante Flexible Assets |

### Konfiguration
- Ticket Category: [Kategorie]
- Aktiviert für Security Levels: [Liste]
- Aktiviert für Departments: [Liste]
```

## ConnectWise Integration

### ConnectWise-Dokumentation
```markdown
## ConnectWise Manage Integration

### API-Konfiguration
| Einstellung | Wert |
|-------------|------|
| Company ID | [ID] |
| Public Key | [Key] |
| Private Key | [gespeichert als Passwort] |
| API URL | [URL] |

### Security Roles
- Erforderliche Rolle für ITGlue API-Zugriff
- Berechtigungen für Tickets, Companies, Configurations

### Pod-Konfiguration
| Pod | Aktiviert |
|-----|-----------|
| Configurations | Ja/Nein |
| Passwords | Ja/Nein |
| Documents | Ja/Nein |
```

## Kaseya BMS Integration

### BMS-Dokumentation
```markdown
## Kaseya BMS Integration

### API-Konfiguration
| Einstellung | Wert |
|-------------|------|
| API Key | [gespeichert als Passwort] |
| Tenant | [Tenant-Name] |

### Two-Way Sync (BMS-spezifisch)
- Kontakt-Sync: Ja/Nein
- Organisationen-Sync: Ja/Nein
- Konfigurationen-Sync: Ja/Nein
```

## Two-Way Sync

### Two-Way Sync Dokumentation
```markdown
## Two-Way Sync: [PSA-Name]

### Aktivierte Felder
| Feld | Sync-Richtung |
|------|---------------|
| Organization Name | PSA ↔ ITGlue |
| Contact Email | PSA ↔ ITGlue |
| Contact Phone | PSA ↔ ITGlue |
| Location Address | PSA ↔ ITGlue |

### Sync-Frequenz
- ITGlue → PSA: Sofort nach Änderung
- PSA → ITGlue: Regelmäßig + manuell

### Konfliktbehandlung
- Source of Truth: [PSA/ITGlue]
- Letzte Änderung gewinnt: Ja/Nein
```

## Field Mappings

### Mapping-Dokumentation
```markdown
## Field Mappings: [PSA-Name]

### Organisationen
| PSA Feld | ITGlue Feld |
|----------|-------------|
| Company Name | Organization Name |
| Company ID | External ID |
| Status | Organization Status |

### Kontakte
| PSA Feld | ITGlue Feld |
|----------|-------------|
| First Name | First Name |
| Last Name | Last Name |
| Email | Primary Email |
| Phone | Office Phone |
| Mobile | Mobile Phone |

### Konfigurationen
| PSA Feld | ITGlue Feld |
|----------|-------------|
| Configuration Name | Name |
| Serial Number | Serial Number |
| Asset Tag | Asset Tag |
| Manufacturer | Manufacturer |
| Model | Model |
```

## Sync-Probleme dokumentieren

### Fehlerbehandlung
```markdown
## Sync-Fehlerprotokoll

### Häufige Probleme
| Problem | Ursache | Lösung |
|---------|---------|--------|
| Duplikate | Matching fehlgeschlagen | Manuelles Matching |
| Fehlende Felder | API-Berechtigung | Berechtigungen prüfen |
| Timeout | Zu viele Datensätze | Zeitplan anpassen |

### Monitoring
- Letzte erfolgreiche Sync: [Datum]
- Fehlgeschlagene Syncs: [Anzahl]
- Warnungen: [Details]
```

## Disconnect-Verfahren

### Integration trennen
```markdown
## PSA-Integration trennen

### Option 1: Disconnect mit Datenerhalt
- Verbindung zu PSA wird getrennt
- Alle synchronisierten Daten bleiben in ITGlue
- Daten können mit anderen Integrationen verwendet werden

### Option 2: Disconnect mit Datenlöschung
- WARNUNG: Unwiderruflich!
- Alle synchronisierten Organisationen werden gelöscht
- Passwörter, Dokumente, Flexible Assets werden gelöscht
- Nur über Support aktivierbar

### Vor dem Disconnect
- [ ] Vollständiges Backup erstellen
- [ ] Dokumentation der aktuellen Konfiguration
- [ ] Betroffene Benutzer informieren
```

## Checkliste PSA-Integration

### Einrichtung
- [ ] API-Credentials konfiguriert
- [ ] Berechtigungen geprüft
- [ ] Sync-Einstellungen definiert
- [ ] Two-Way Sync aktiviert (falls gewünscht)
- [ ] Field Mappings dokumentiert

### Laufender Betrieb
- [ ] Sync-Status regelmäßig prüfen
- [ ] Fehlerprotokoll überwachen
- [ ] Matching regelmäßig durchführen
- [ ] Duplikate bereinigen

### Dokumentation
- [ ] API-Credentials sicher gespeichert
- [ ] Konfiguration dokumentiert
- [ ] Ticket Rules dokumentiert (Autotask)
- [ ] LiveLinks/Pods dokumentiert
