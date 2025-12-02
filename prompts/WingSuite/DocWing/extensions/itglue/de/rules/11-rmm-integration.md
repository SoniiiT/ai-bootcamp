# Regel 11: RMM-Integration

## Zweck
Diese Regel definiert Standards für die Dokumentation von RMM-Integrationen (Datto RMM, VSA, ConnectWise RMM).

## Allgemeine RMM-Dokumentation

### Integration-Übersicht
```markdown
## RMM-Integration: [RMM-Name]

### Verbindungsinformationen
| Eigenschaft | Wert |
|-------------|------|
| RMM-Typ | Datto RMM/VSA/ConnectWise RMM |
| API-Endpoint | [URL] |
| Sync-Status | Aktiv/Pausiert |
| Letzte Synchronisation | [Datum/Zeit] |

### Sync-Verhalten
- RMM-Daten werden OVERLAY angezeigt
- Keine Schreiboperationen in ITGlue
- "Compare Data" zum Vergleich verfügbar
```

## Datto RMM Integration

### Datto RMM Dokumentation
```markdown
## Datto RMM Integration

### API-Konfiguration
| Einstellung | Wert |
|-------------|------|
| API URL | [URL] |
| API Key | [gespeichert als Passwort] |
| API Secret | [gespeichert als Passwort] |

### Sync-Einstellungen
| Datentyp | Aktiviert |
|----------|-----------|
| Sites → Organisationen | Ja/Nein |
| Devices → Konfigurationen | Ja/Nein |

### SOP Generator Integration
- Web Remote Sessions werden erfasst
- Automatische SOP-Erstellung möglich
```

## VSA Integration

### VSA-Dokumentation
```markdown
## Kaseya VSA Integration

### API-Konfiguration
| Einstellung | Wert |
|-------------|------|
| VSA URL | [URL] |
| Authentication | PAT (Personal Access Token) |

### Personal Access Token (PAT)
| Einstellung | Wert |
|-------------|------|
| Token Name | [Name] |
| Scope | REST API Read/Write |
| IP Whitelist | [IPs oder leer] |
| Ablaufdatum | [Datum] |

### Agent Procedures
- Aufruf von ITGlue-Daten aus Procedures möglich
- URL-Parameterübergabe
```

## Field Mappings

### RMM → ITGlue Mappings
```markdown
## Field Mappings: [RMM-Name]

### Konfigurationen
| RMM Feld | ITGlue Feld |
|----------|-------------|
| Device Name | Hostname |
| Serial Number | Serial Number |
| MAC Address | Primary MAC |
| IP Address | Primary IP |
| Manufacturer | Manufacturer |
| Model | Model |
| OS | Operating System |
| Last Reboot | Last Reboot |
| CPU | CPU |
| RAM | RAM |
| Last Check-in | Last Check-in |
```

## Device Matching

### Matching-Logik
```markdown
## Device Matching: [RMM-Name]

### Automatisches Matching
| Priorität | Kriterium |
|-----------|-----------|
| 1 | MAC-Adresse + Seriennummer |
| 2 | Nur MAC-Adresse |
| 3 | Nur Seriennummer |

### Manuelles Matching
- Geräte mit ähnlichen Namen werden vorgeschlagen
- Match/Ignore/Create Aktionen verfügbar

### Matching-Status
| Status | Bedeutung |
|--------|-----------|
| Matched | Erfolgreich verknüpft |
| Suggested | Vorschlag verfügbar |
| Unmatched | Keine Übereinstimmung |
| Ignored | Bewusst übersprungen |
```

## Compare Data Feature

### Datenvergleich dokumentieren
```markdown
## Compare Data: Konfigurationen

### Verfügbare Felder für Vergleich
| Feld | ITGlue Wert | RMM Wert |
|------|-------------|----------|
| Hostname | [Wert] | [Wert] |
| Serial | [Wert] | [Wert] |
| IP Address | [Wert] | [Wert] |

### Hinweis
- RMM-Daten werden nicht in ITGlue geschrieben
- Werden als Overlay angezeigt
- Bei Abweichungen: RMM-Wert wird bevorzugt angezeigt
```

## Datto Networking

### Datto Networking Dokumentation
```markdown
## Datto Networking Integration

### Funktionen
- Wireless SSID Auto-Documentation
- Netzwerkgeräte-Sync
- Access Point-Dokumentation

### Wireless Networks
| SSID | Security Type | Passwort |
|------|---------------|----------|
| [SSID] | WPA2-Personal | [Als ITGlue Passwort] |

### Flexible Asset
- Typ: Datto Networking Wireless
- Automatisch erstellt bei WPA Personal/PSK
```

## Datto BCDR Integration

### BCDR-Dokumentation
```markdown
## Datto BCDR Integration

### Funktionen
- DR Runbook Zugriff von Organisations-Home
- Backup-Status-Anzeige
- Gerätezuordnung

### Matching
- BCDR-Geräte → ITGlue-Konfigurationen
- Organisations-Level Zuordnung

### Quick Access
- DR Runbook Link auf Organization Home
- Nur für gematchte Organisationen sichtbar
```

## Datto Endpoint Backup

### Endpoint Backup Dokumentation
```markdown
## Datto Endpoint Backup

### Funktionen
- Backup-Status für PCs
- Organisations-Sync
- Coverage-Anzeige

### Integration
- SaaS Protection Coverage in Org-Liste
- Product Type Anzeige
```

## Organisation Matching

### Organisations-Matching
```markdown
## Organisation Matching: [RMM-Name]

### Automatisches Matching
- Exakter Name-Match erforderlich
- Pattern Recognition für Vorschläge

### Matching-Dokumentation
| RMM-Org | ITGlue-Org | Status | Methode |
|---------|-----------|--------|---------|
| [Name] | [Name] | Matched | Auto/Manual |

### Best Practices
- PSA zuerst integrieren
- RMM-Daten matchen nicht erstellen
- Duplikate vermeiden
```

## Sync-Methoden

### Empfohlene Methode
```markdown
## Sync-Strategie: PSA + RMM

### Option B (Empfohlen)
1. RMM mit PSA integrieren
2. RMM-Daten in PSA pushen
3. PSA mit ITGlue integrieren
4. RMM mit ITGlue integrieren
5. RMM-Daten als Overlay nutzen

### Vorteile
- Keine Duplikate
- Automatisches Matching
- Kombinierte Datensicht
```

## Checkliste RMM-Integration

### Einrichtung
- [ ] API-Credentials konfiguriert
- [ ] PAT Token erstellt (VSA)
- [ ] Sync-Einstellungen definiert
- [ ] Organisations-Matching durchgeführt
- [ ] Device-Matching durchgeführt

### Laufender Betrieb
- [ ] Sync-Status prüfen
- [ ] Neue Geräte matchen
- [ ] Orphaned Devices bereinigen
- [ ] Compare Data regelmäßig nutzen

### Dokumentation
- [ ] API-Credentials gespeichert
- [ ] Field Mappings dokumentiert
- [ ] Matching-Regeln dokumentiert
- [ ] Token-Ablauf notiert (VSA)
