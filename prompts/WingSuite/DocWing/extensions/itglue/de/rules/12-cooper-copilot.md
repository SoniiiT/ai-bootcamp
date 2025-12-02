# Regel 12: Cooper Copilot Features

## Zweck
Diese Regel definiert Standards für die Dokumentation der Cooper Copilot AI-Features in ITGlue.

## Cooper Copilot Übersicht

### Feature-Dokumentation
```markdown
## Cooper Copilot: Konfigurationsübersicht

### Aktivierungsstatus
| Feature | Status |
|---------|--------|
| Cooper Copilot | Aktiviert/Deaktiviert |
| Smart Relate | Aktiviert/Deaktiviert |
| Smart SOP Generator | Aktiviert/Deaktiviert |
| Smart Assist | Aktiviert/Deaktiviert |

### Konfiguration
- Admin > Settings > Cooper Copilot
- Toggle für Aktivierung/Deaktivierung
```

## Smart SOP Generator

### SOP Generator Dokumentation
```markdown
## Cooper Copilot Smart SOP Generator

### Funktionsbeschreibung
Automatische Erstellung von Standard Operating Procedures (SOPs) durch Aufzeichnung von Browser-Aktivitäten.

### Voraussetzungen
- ITGlue Chrome Extension installiert
- Cooper Copilot aktiviert
- Benutzer in ITGlue eingeloggt

### Unterstützte Aktionen
- Browser-Navigation
- Klicks auf Elemente
- Texteingaben
- Datto RMM Web Remote Sessions
- Manuelle Screenshots

### Workflow
1. Extension öffnen
2. SOP Generator starten
3. Aktionen durchführen
4. Vorschau prüfen
5. Sensible Informationen ausblenden
6. Dokument in ITGlue speichern
```

### SOP-Dokumentationsvorlage
```markdown
## SOP: [Prozessname]

### Übersicht
| Eigenschaft | Wert |
|-------------|------|
| Erstellt am | [Datum] |
| Erstellt mit | Cooper Copilot SOP Generator |
| Zielgruppe | [Benutzergruppe] |
| Organisation | [Organisation] |

### Schritte
[Automatisch generierte Schritte mit Screenshots]

### Hinweise
- Sensible Daten wurden ausgeblendet
- Screenshots wurden bearbeitet (falls zutreffend)
```

## Smart Relate

### Smart Relate Dokumentation
```markdown
## Cooper Copilot Smart Relate

### Funktionsbeschreibung
Automatische Verknüpfung von zusammengehörigen Assets mittels AI.

### Unterstützte Verknüpfungen
| Quelle | Ziel | Beispiel |
|--------|------|----------|
| Intune Device | M365 User | Gerät ↔ Zugewiesener Benutzer |
| Configuration | Contact | Server ↔ Administrator |

### Voraussetzungen
- Microsoft Integration aktiviert
- Device Sync aktiviert
- Contact Sync aktiviert

### Manuelle Ergänzungen
- Automatische Verknüpfungen können bearbeitet werden
- Zusätzliche Related Items manuell hinzufügbar
```

## Smart Assist

### Smart Assist Dokumentation
```markdown
## Cooper Copilot Smart Assist

### Funktionsbeschreibung
AI-gestützte Analyse und Bereinigung von Dokumenten.

### Funktionen
| Feature | Beschreibung |
|---------|--------------|
| Stale Documents | Identifiziert veraltete Dokumente |
| Redundant Documents | Findet Duplikate |
| Doc Health | Bewertet Dokumentenqualität |

### Cleanup-Empfehlungen
- Veraltete Dokumente archivieren
- Duplikate zusammenführen
- Fehlende Inhalte ergänzen

### Aktionen
| Empfehlung | Aktion |
|------------|--------|
| Veraltet | Archivieren/Aktualisieren |
| Duplikat | Zusammenführen/Löschen |
| Unvollständig | Ergänzen |
```

## @relate Links in Dokumenten

### @relate Dokumentation
```markdown
## @relate Funktionalität

### Verwendung
1. In Dokument-Textfeld "@relate" eingeben
2. Asset-Typ auswählen
3. Global oder pro Organisation filtern
4. Asset auswählen
5. Verknüpfung wird erstellt

### Unterstützte Asset-Typen
- Konfigurationen
- Passwörter
- Flexible Assets
- Dokumente
- Kontakte

### Beispiel
@relate [Configuration: Server01] erstellt automatische Verknüpfung
```

## Cooper Insights (KaseyaOne)

### KaseyaOne Cooper Insights
```markdown
## Cooper Insights: Password SafeShare

### Beschreibung
Cooper Insight zur Förderung der SafeShare-Nutzung.

### Kriterien
- Insight wird angezeigt wenn: SafeShare nicht aktiviert
- Insight ist abgeschlossen wenn: Mindestens ein Passwort geteilt

### Weitere Insights
[Dokumentation weiterer Cooper Insights hier]
```

## Feature-Deaktivierung

### Deaktivierung dokumentieren
```markdown
## Cooper Copilot Deaktivierung

### Gründe für Deaktivierung
- Datenschutzbedenken
- Compliance-Anforderungen
- Manuelle Kontrolle bevorzugt

### Vorgehensweise
1. Admin > Settings > Cooper Copilot
2. Toggle auf "Aus" setzen
3. Speichern

### Auswirkungen
- Smart Relate deaktiviert
- SOP Generator nicht verfügbar
- Smart Assist nicht verfügbar
- Bestehende Verknüpfungen bleiben erhalten
```

## Best Practices

### Cooper Copilot Best Practices
```markdown
## Best Practices: Cooper Copilot

### SOP Generator
- Testlauf vor Produktivnutzung
- Sensible Daten vor Veröffentlichung prüfen
- Screenshots auf Sichtbarkeit prüfen
- Regelmäßige SOP-Updates planen

### Smart Relate
- Automatische Verknüpfungen regelmäßig prüfen
- Manuelle Korrekturen bei Fehlverknüpfungen
- Integration-Voraussetzungen sicherstellen

### Smart Assist
- Regelmäßige Cleanup-Zyklen einplanen
- Archivierung vor Löschung bevorzugen
- Team über Bereinigungsaktionen informieren
```

## Checkliste Cooper Copilot

### Aktivierung
- [ ] Cooper Copilot aktiviert
- [ ] Team über Features informiert
- [ ] Datenschutz-Review durchgeführt

### SOP Generator
- [ ] Chrome Extension installiert
- [ ] Testdokument erstellt
- [ ] Workflow getestet

### Smart Relate
- [ ] Microsoft Integration aktiviert
- [ ] Verknüpfungen geprüft

### Smart Assist
- [ ] Stale Documents Review geplant
- [ ] Cleanup-Strategie definiert
