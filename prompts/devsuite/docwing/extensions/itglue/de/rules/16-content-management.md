# Regel 16: Content-Management

## Zweck
Diese Regel definiert Standards für die Dokumentation von Content-Management-Funktionen in ITGlue, einschließlich Bulk-Operationen, GlueConnect und Checklisten.

## Bulk-Operationen

### Bulk-Operationen dokumentieren
```markdown
## ITGlue Bulk-Operationen

### Verfügbare Bulk-Aktionen
| Aktion | Beschreibung | Asset-Typen |
|--------|--------------|-------------|
| Bulk Edit | Mehrere Einträge gleichzeitig bearbeiten | Alle |
| Bulk Delete | Mehrere Einträge löschen | Alle |
| Bulk Move | In andere Organisation verschieben | Configs, Docs |
| Bulk Copy | Kopieren zu anderer Organisation | Docs |
| Bulk Tag | Tags hinzufügen/entfernen | Alle |
| Bulk Archive | Archivieren | Alle |

### Vorgehensweise
1. Zur Asset-Liste navigieren
2. Einträge per Checkbox auswählen
3. "Bulk Actions" Menü öffnen
4. Aktion auswählen
5. Bestätigen
```

### Bulk-Edit Dokumentation
```markdown
## Bulk Edit: Massenbearbeitung

### Unterstützte Felder
| Asset-Typ | Bearbeitbare Felder |
|-----------|---------------------|
| Configurations | Status, Type, Tags, Location |
| Contacts | Type, Tags, Location |
| Passwords | Category, Tags |
| Documents | Folder, Tags |

### Limitierungen
- Maximale Anzahl pro Bulk: [Limit]
- Nicht alle Felder bulk-editierbar
- Flexible Asset Fields: Eingeschränkt
```

## GlueConnect

### GlueConnect Dokumentation
```markdown
## GlueConnect: Dokumentenverknüpfung

### Funktionsbeschreibung
GlueConnect ermöglicht die Verknüpfung von Dokumenten über Organisationen hinweg.

### Verwendungsfälle
| Szenario | Beschreibung |
|----------|--------------|
| MSP-interne Docs | Dokumentation für alle Kunden teilen |
| Vorlagen | Template-Dokumente verteilen |
| SOPs | Standard-Prozesse teilen |
| Compliance | Richtlinien-Dokumente |

### Konfiguration
1. Dokument in "Global" Organisation erstellen
2. GlueConnect für Dokument aktivieren
3. Ziel-Organisationen auswählen
4. Synchronisierung starten
```

### GlueConnect-Vorlage
```markdown
## GlueConnect-Dokument: [Dokumentname]

### Metadaten
| Eigenschaft | Wert |
|-------------|------|
| Quell-Organisation | Global / [Name] |
| Verknüpfte Orgs | [Anzahl] |
| Letzte Sync | [Datum] |
| Status | Aktiv/Inaktiv |

### Verknüpfte Organisationen
| Organisation | Status | Letzte Sync |
|--------------|--------|-------------|
| [Org 1] | Synchronisiert | [Datum] |
| [Org 2] | Synchronisiert | [Datum] |

### Änderungshistorie
| Datum | Änderung | Autor |
|-------|----------|-------|
| [Datum] | [Beschreibung] | [Name] |
```

## Checklisten

### Checklisten in Dokumenten
```markdown
## ITGlue Checklisten

### Checklisten-Typen
| Typ | Verwendung |
|-----|------------|
| Onboarding | Neukunden-Setup |
| Offboarding | Kunden-Ausstieg |
| Server-Setup | Server-Deployment |
| Security-Review | Sicherheitsüberprüfung |
| Maintenance | Wartungsarbeiten |

### Checklisten-Format in Dokumenten
```markdown
## Checkliste: [Name]

### Phase 1: [Phasenname]
- [ ] Aufgabe 1
- [ ] Aufgabe 2
  - [ ] Unteraufgabe 2.1
  - [ ] Unteraufgabe 2.2
- [ ] Aufgabe 3

### Phase 2: [Phasenname]
- [ ] Aufgabe 4
- [ ] Aufgabe 5
```

### Checklisten-Tracking
| Eigenschaft | Wert |
|-------------|------|
| Fortschritt | X/Y abgeschlossen |
| Letzte Aktualisierung | [Datum] |
| Bearbeiter | [Name] |
```

## Dokument-Ordner

### Ordnerstruktur dokumentieren
```markdown
## Dokumenten-Ordner: [Organisationsname]

### Ordnerstruktur
```
📁 Root
├── 📁 Allgemein
│   ├── 📁 Kontakte
│   └── 📁 Verträge
├── 📁 Technisch
│   ├── 📁 Netzwerk
│   ├── 📁 Server
│   └── 📁 Workstations
├── 📁 Prozesse
│   ├── 📁 SOPs
│   └── 📁 Checklisten
└── 📁 Archiv
```

### Ordner-Richtlinien
| Regel | Beschreibung |
|-------|--------------|
| Benennung | [Konvention] |
| Max. Tiefe | 3 Ebenen empfohlen |
| Archivierung | Nach [Zeitraum] |
```

### Ordner-Best-Practices
```markdown
## Best Practices: Ordnerstruktur

### Empfehlungen
- Konsistente Struktur über alle Orgs
- Nicht zu tief verschachteln
- Beschreibende Namen
- Archiv-Ordner für alte Dokumente

### Standard-Struktur (MSP)
| Ordner | Inhalt |
|--------|--------|
| /General | Übersichtsdokumente |
| /Network | Netzwerk-Dokumentation |
| /Servers | Server-Dokumentation |
| /Workstations | Client-Dokumentation |
| /Applications | Anwendungen |
| /Procedures | SOPs und Prozesse |
| /Archive | Archivierte Dokumente |
```

## Dokument-Kopieren & Verschieben

### Copy/Move Dokumentation
```markdown
## Dokumente kopieren und verschieben

### Kopieren
| Eigenschaft | Beschreibung |
|-------------|--------------|
| Aktion | Erstellt Kopie in Ziel |
| Original | Bleibt erhalten |
| Verknüpfungen | Werden nicht kopiert |
| Embedded Passwords | Optional kopieren |

### Verschieben
| Eigenschaft | Beschreibung |
|-------------|--------------|
| Aktion | Verschiebt zu Ziel |
| Original | Wird entfernt |
| Verknüpfungen | Werden aufgelöst |
| Embedded Passwords | Werden verschoben |

### Vorgehensweise
1. Dokument auswählen
2. Actions > Copy/Move
3. Ziel-Organisation wählen
4. Ziel-Ordner wählen
5. Bestätigen
```

## Tags

### Tag-Management dokumentieren
```markdown
## ITGlue Tags: Übersicht

### Tag-Kategorien
| Kategorie | Tags | Verwendung |
|-----------|------|------------|
| Status | Active, Inactive, Pending | Asset-Status |
| Priority | Critical, High, Normal, Low | Wichtigkeit |
| Type | Server, Workstation, Network | Asset-Typ |
| Location | HQ, Branch, Remote | Standort |
| Compliance | PCI, HIPAA, SOC2 | Compliance |

### Tag-Richtlinien
- Einheitliche Benennung (CamelCase)
- Keine Duplikate
- Regelmäßige Bereinigung
- Dokumentation aller Tags
```

### Tag-Filter nutzen
```markdown
## Tag-basierte Filterung

### Filter-Beispiele
| Filter | Ergebnis |
|--------|----------|
| tag:Critical | Alle kritischen Assets |
| tag:Server AND tag:Production | Produktions-Server |
| -tag:Archived | Nicht archivierte |

### Dashboards mit Tags
- Dashboard nach Tags erstellen
- Tag-Statistiken anzeigen
- Schnellzugriff auf Tag-gefilterte Listen
```

## Dokument-Revisionen

### Revisions-Dokumentation
```markdown
## Dokumenten-Revisionen

### Revisionsverlauf
| Version | Datum | Autor | Änderung |
|---------|-------|-------|----------|
| v3 | [Datum] | [Name] | [Beschreibung] |
| v2 | [Datum] | [Name] | [Beschreibung] |
| v1 | [Datum] | [Name] | Initiale Version |

### Revision anzeigen
1. Dokument öffnen
2. "History" klicken
3. Version auswählen
4. Vergleichen oder Wiederherstellen

### Wiederherstellung
- Jede Version kann wiederhergestellt werden
- Erstellt neue Version basierend auf alter
- Änderungen werden protokolliert
```

## Archivierung

### Archivierungs-Richtlinien
```markdown
## Archivierung: Richtlinien

### Wann archivieren
| Situation | Aktion |
|-----------|--------|
| Projekt abgeschlossen | Dokumente archivieren |
| Kunde inaktiv | Alle Assets archivieren |
| Veraltete Information | Dokument archivieren |
| Nach Retention | Automatisch archivieren |

### Archivierungs-Prozess
1. Asset auswählen
2. Status auf "Archived" setzen
3. In Archiv-Ordner verschieben
4. Archivierungs-Tag hinzufügen

### Auffinden archivierter Inhalte
- Filter: Status = Archived
- Suche in Archiv-Ordnern
- Include Archived in Suche aktivieren
```

## Content-Lifecycle

### Lifecycle-Dokumentation
```markdown
## Content-Lifecycle-Management

### Lifecycle-Phasen
| Phase | Status | Aktion |
|-------|--------|--------|
| Entwurf | Draft | In Bearbeitung |
| Review | Needs Review | Prüfung erforderlich |
| Aktiv | Published | Live und aktuell |
| Veraltet | Outdated | Aktualisierung nötig |
| Archiviert | Archived | Nicht mehr aktiv |

### Automatische Trigger
| Alter | Aktion |
|-------|--------|
| 90 Tage | Review-Reminder |
| 180 Tage | Outdated-Flag |
| 365 Tage | Archivierungs-Vorschlag |

### Review-Zyklus
- Vierteljährliche Review aller kritischen Docs
- Jährliche Review aller Dokumente
- Automatische Benachrichtigungen
```

## Best Practices Content-Management

### Best Practices
```markdown
## Content-Management: Best Practices

### Organisation
- Konsistente Ordnerstruktur
- Einheitliche Benennungen
- Tags konsequent nutzen
- Regelmäßige Aufräumarbeiten

### Qualität
- Templates verwenden
- Review-Prozess einhalten
- Veraltetes archivieren
- Links aktuell halten

### Zusammenarbeit
- GlueConnect für gemeinsame Docs
- Zuständigkeiten definieren
- Änderungen dokumentieren
- Feedback-Prozess etablieren
```

## Checkliste Content-Management

### Ordnerstruktur
- [ ] Standardstruktur definiert
- [ ] Für alle Orgs angewendet
- [ ] Dokumentiert

### Tags
- [ ] Tag-Katalog erstellt
- [ ] Richtlinien dokumentiert
- [ ] Team geschult

### Lifecycle
- [ ] Review-Zyklen definiert
- [ ] Archivierungs-Richtlinien
- [ ] Automatisierung eingerichtet

### GlueConnect
- [ ] Gemeinsame Docs identifiziert
- [ ] GlueConnect konfiguriert
- [ ] Sync-Status überwacht
