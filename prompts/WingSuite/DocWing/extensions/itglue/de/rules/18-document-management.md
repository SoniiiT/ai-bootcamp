# Regel 18: Dokumenten- & Asset-Management

## Zweck
Diese Regel definiert Standards für die Dokumentation von Dokumenten- und Asset-Management-Funktionen in ITGlue.

## Dokumenten-Erstellung

### Dokument-Typen
```markdown
## ITGlue Dokument-Typen

### Standard-Dokumente
| Typ | Verwendung |
|-----|------------|
| Eingebettet | Im ITGlue-Editor erstellt |
| Verknüpft | Externe Datei (PDF, Word, etc.) |
| Template | Wiederverwendbare Vorlage |

### Dokumenten-Struktur
| Element | Beschreibung |
|---------|--------------|
| Titel | Beschreibender Name |
| Ordner | Organisatorische Zuordnung |
| Tags | Kategorisierung |
| Content | Rich-Text-Inhalt |
| Attachments | Dateianhänge |
| Related Items | Verknüpfte Assets |
```

### Dokumenten-Vorlage
```markdown
## Dokument: [Dokumentname]

### Metadaten
| Eigenschaft | Wert |
|-------------|------|
| Organisation | [Organisationsname] |
| Ordner | [Ordnerpfad] |
| Erstellt | [Datum] |
| Autor | [Name] |
| Letzte Änderung | [Datum] |
| Status | [Draft/Published/Archived] |

### Tags
[Tag1], [Tag2], [Tag3]

### Inhalt
[Dokumenten-Inhalt hier]

### Related Items
- [Asset-Typ]: [Asset-Name]
- [Asset-Typ]: [Asset-Name]
```

## Sub-Dokumente

### Sub-Dokument-Struktur
```markdown
## Sub-Dokumente: Hierarchische Dokumentation

### Verwendung
Sub-Dokumente ermöglichen geschachtelte Dokumentenstrukturen.

### Struktur-Beispiel
```
📄 Hauptdokument: Netzwerk-Übersicht
├── 📄 Sub-Doc: Router-Konfiguration
├── 📄 Sub-Doc: Switch-Konfiguration
│   ├── 📄 Sub-Doc: VLAN-Setup
│   └── 📄 Sub-Doc: Port-Zuweisungen
└── 📄 Sub-Doc: Firewall-Regeln
```

### Best Practices
| Regel | Beschreibung |
|-------|--------------|
| Tiefe | Max. 3 Ebenen empfohlen |
| Benennung | Klare, beschreibende Namen |
| Vererbung | Berechtigungen werden vererbt |
```

## Rich-Text-Editor

### Editor-Funktionen dokumentieren
```markdown
## ITGlue Rich-Text-Editor

### Formatierungsoptionen
| Funktion | Tastenkürzel | Beschreibung |
|----------|--------------|--------------|
| Fett | Ctrl+B | Text hervorheben |
| Kursiv | Ctrl+I | Text kursiv |
| Überschrift | - | H1, H2, H3 |
| Liste | - | Aufzählung/Nummerierung |
| Tabelle | - | Tabelleneinbindung |
| Code | - | Code-Block |
| Link | Ctrl+K | Hyperlink |

### Eingebettete Elemente
| Element | Beschreibung |
|---------|--------------|
| Bilder | Drag & Drop oder Upload |
| Screenshots | Direktes Einfügen |
| Diagramme | Externe Bilder verlinken |
| Videos | Eingebettete Links |
```

### @relate-Funktion
```markdown
## @relate: Asset-Verknüpfung im Text

### Verwendung
1. "@relate" im Text eingeben
2. Asset-Typ auswählen
3. Asset suchen und auswählen
4. Verknüpfung wird erstellt

### Unterstützte Typen
- @relate Configuration
- @relate Password
- @relate Document
- @relate Contact
- @relate Flexible Asset

### Vorteile
- Automatische Verlinkung
- Related Items werden aktualisiert
- Bidirektionale Verknüpfung
```

## Revisionsverwaltung

### Revisions-Dokumentation
```markdown
## Dokumenten-Revisionen

### Automatische Versionierung
| Trigger | Beschreibung |
|---------|--------------|
| Speichern | Neue Version bei jeder Änderung |
| Zeitbasiert | Zusammenfassung nach Inaktivität |

### Versions-Ansicht
| Spalte | Beschreibung |
|--------|--------------|
| Version | Versionsnummer |
| Datum | Änderungszeitpunkt |
| Autor | Bearbeiter |
| Änderungen | Diff-Ansicht |

### Wiederherstellen
1. "History" öffnen
2. Version auswählen
3. "Restore" klicken
4. Neue Version basierend auf alter wird erstellt
```

### Vergleichs-Funktion
```markdown
## Versionen vergleichen

### Diff-Ansicht
| Anzeige | Bedeutung |
|---------|-----------|
| Grün | Hinzugefügt |
| Rot | Entfernt |
| Gelb | Geändert |

### Vergleich durchführen
1. History öffnen
2. Zwei Versionen auswählen
3. "Compare" klicken
4. Unterschiede analysieren
```

## Audit-Trail

### Audit-Dokumentation
```markdown
## Dokumenten-Audit-Trail

### Protokollierte Aktionen
| Aktion | Details |
|--------|---------|
| Created | Erstellung mit Autor |
| Updated | Änderung mit Benutzer |
| Viewed | Zugriff (optional) |
| Deleted | Löschung mit Benutzer |
| Restored | Wiederherstellung |
| Permission Changed | Berechtigungsänderung |

### Audit-Report
| Spalte | Beschreibung |
|--------|--------------|
| Timestamp | Zeitpunkt der Aktion |
| User | Durchführender Benutzer |
| Action | Art der Aktion |
| Details | Zusätzliche Informationen |

### Aufbewahrung
- Audit-Logs werden [X] Tage gespeichert
- Export für Compliance möglich
```

## Asset-Management

### Konfigurationen verwalten
```markdown
## Konfigurations-Management

### Konfigurations-Felder
| Feld | Beschreibung | Pflicht |
|------|--------------|---------|
| Name | Asset-Name | ✅ |
| Configuration Type | Server, Workstation, etc. | ✅ |
| Configuration Status | Active, Inactive, etc. | ✅ |
| Organization | Zugehörige Org | ✅ |
| Location | Standort | Optional |
| Serial Number | Seriennummer | Optional |
| Notes | Zusätzliche Infos | Optional |

### Konfigurationstypen
- Server
- Workstation
- Network Device
- Mobile Device
- Virtual Machine
- Printer
- [Custom Types]
```

### Asset-Lifecycle
```markdown
## Asset-Lifecycle-Management

### Status-Workflow
| Status | Beschreibung |
|--------|--------------|
| Active | In Produktion |
| Inactive | Nicht in Nutzung |
| Decommissioned | Außer Betrieb |
| In Repair | In Reparatur |
| Pending | Wartet auf Setup |

### Lifecycle-Dokumentation
| Phase | Aktion | Dokumentation |
|-------|--------|---------------|
| Beschaffung | Asset anlegen | Basis-Infos |
| Deployment | In Betrieb nehmen | Konfig-Details |
| Betrieb | Laufende Nutzung | Updates, Changes |
| Wartung | Maintenance | Wartungsprotokolle |
| Ausmusterung | Dekommissionierung | Archivierung |
```

## Flexible Assets

### Flexible Asset dokumentieren
```markdown
## Flexible Asset: [Asset-Typ-Name]

### Typ-Definition
| Eigenschaft | Wert |
|-------------|------|
| Name | [Asset-Typ-Name] |
| Icon | [Icon-Name] |
| Beschreibung | [Beschreibung] |
| Tracking | Aktiviert/Deaktiviert |

### Felder
| Feldname | Typ | Pflicht | Beschreibung |
|----------|-----|---------|--------------|
| [Feld 1] | Text | ✅ | [Beschreibung] |
| [Feld 2] | Number | ❌ | [Beschreibung] |
| [Feld 3] | Select | ✅ | [Optionen] |
| [Feld 4] | Checkbox | ❌ | [Beschreibung] |
| [Feld 5] | Date | ❌ | [Beschreibung] |
| [Feld 6] | Text Area | ❌ | [Beschreibung] |
| [Feld 7] | Upload | ❌ | [Beschreibung] |
| [Feld 8] | Password | ❌ | Embedded Password |
| [Feld 9] | Tag | ❌ | [Tag-Optionen] |

### Verknüpfungen
- Organisationen: [Ja/Nein]
- Konfigurationen: [Ja/Nein]
- Kontakte: [Ja/Nein]
- Andere Flexible Assets: [Liste]
```

## Passwort-Management (Assets)

### Passwort-Kategorien
```markdown
## Passwort-Kategorien

### Standard-Kategorien
| Kategorie | Verwendung |
|-----------|------------|
| Admin | Administrator-Zugänge |
| User | Benutzer-Zugänge |
| Service | Service-Accounts |
| API | API-Keys |
| WiFi | WLAN-Passwörter |
| Encryption | Verschlüsselungs-Keys |

### Kategorie-basierte Berechtigungen
| Kategorie | IT-Team | Management | Kunden |
|-----------|---------|------------|--------|
| Admin | ✅ | ❌ | ❌ |
| User | ✅ | ✅ | ✅ |
| Service | ✅ | ❌ | ❌ |
| API | ✅ | ❌ | ❌ |
```

### Embedded Passwords
```markdown
## Embedded Passwords

### Beschreibung
Passwörter direkt in Dokumenten oder Flexible Assets einbetten.

### Verwendung
1. Im Editor "Embedded Password" einfügen
2. Passwort-Details eingeben
3. Position im Dokument wählen
4. Speichern

### Sicherheit
- Embedded Passwords erben Dokument-Berechtigungen
- Separate Audit-Logs
- Können separat exportiert werden
```

## Kontakt-Management

### Kontakte dokumentieren
```markdown
## Kontakt-Management

### Kontakt-Felder
| Feld | Beschreibung |
|------|--------------|
| Name | Vollständiger Name |
| Title | Titel/Position |
| Email | E-Mail-Adresse |
| Phone | Telefonnummer |
| Contact Type | IT, Management, etc. |
| Location | Zugeordneter Standort |
| Organization | Zugehörige Org |

### Kontakt-Typen
| Typ | Beschreibung |
|-----|--------------|
| Primary Contact | Hauptansprechpartner |
| Technical Contact | Technischer Kontakt |
| Billing Contact | Rechnungs-Kontakt |
| Emergency Contact | Notfall-Kontakt |
```

## Best Practices

### Asset-Management Best Practices
```markdown
## Best Practices: Asset-Management

### Dokumentation
- Vollständige Basis-Daten pflegen
- Regelmäßige Updates
- Related Items verknüpfen
- Tags konsequent nutzen

### Lifecycle
- Status aktuell halten
- Dekommissionierung dokumentieren
- Archivierung statt Löschung

### Qualität
- Templates verwenden
- Naming Conventions einhalten
- Review-Prozess etablieren
```

## Checkliste Dokumenten-Management

### Erstellung
- [ ] Dokumententyp gewählt
- [ ] Ordner zugewiesen
- [ ] Tags hinzugefügt
- [ ] Related Items verknüpft

### Qualität
- [ ] Inhalt vollständig
- [ ] Formatierung korrekt
- [ ] Bilder/Screenshots eingefügt
- [ ] @relate Verknüpfungen

### Lifecycle
- [ ] Erstelldatum dokumentiert
- [ ] Review-Datum geplant
- [ ] Verantwortlicher benannt

### Sicherheit
- [ ] Berechtigungen geprüft
- [ ] Embedded Passwords gesichert
- [ ] Audit-Trail aktiv
