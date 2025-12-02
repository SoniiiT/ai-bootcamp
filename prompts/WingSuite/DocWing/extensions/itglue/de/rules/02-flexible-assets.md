# Regel 2 - Flexible Asset Templates

## Übersicht

Flexible Assets sind benutzerdefinierte Dokumentationsvorlagen in ITGlue, die strukturierte Informationen in wiederholbarer Form speichern.

## Standard-Templates

### Application Template
```yaml
Template Name: Application

Felder:
  - Name: Name
    Typ: Text (Pflichtfeld)
    
  - Name: Hersteller
    Typ: Text
    
  - Name: Version
    Typ: Text
    
  - Name: Beschreibung
    Typ: Textbox
    
  - Name: Installationstyp
    Typ: Select
    Optionen: [On-Premise, Cloud, Hybrid]
    
  - Name: Lizenztyp
    Typ: Select
    Optionen: [Subscription, Perpetual, Open Source, Freeware]
    
  - Name: Lizenzschlüssel
    Typ: Password (verschlüsselt)
    
  - Name: Application Server
    Typ: Tag (Configurations)
    
  - Name: Verantwortlicher
    Typ: Tag (Contacts)
    
  - Name: Support-URL
    Typ: URL
    
  - Name: Dokumentation
    Typ: Textbox
    
  - Name: Notizen
    Typ: Textbox
```

### LAN/Netzwerk Template
```yaml
Template Name: LAN

Felder:
  - Name: Netzwerkname
    Typ: Text (Pflichtfeld)
    
  - Name: Netzwerktyp
    Typ: Select
    Optionen: [LAN, WAN, WLAN, VPN, DMZ]
    
  - Name: Subnetz
    Typ: Text
    Beispiel: 192.168.1.0/24
    
  - Name: Gateway
    Typ: Text
    
  - Name: VLAN ID
    Typ: Number
    
  - Name: DHCP Server
    Typ: Tag (Configurations)
    
  - Name: DNS Server
    Typ: Tag (Configurations)
    
  - Name: Firewall
    Typ: Tag (Configurations)
    
  - Name: Switches
    Typ: Tag (Configurations, Multiple)
    
  - Name: Beschreibung
    Typ: Textbox
    
  - Name: Netzwerkdiagramm
    Typ: Upload
```

### Backup Template
```yaml
Template Name: Backup

Felder:
  - Name: Backup-Name
    Typ: Text (Pflichtfeld)
    
  - Name: Backup-Lösung
    Typ: Select
    Optionen: [Datto SIRIS, Veeam, Acronis, Azure Backup, etc.]
    
  - Name: Backup-Typ
    Typ: Select
    Optionen: [Full, Incremental, Differential, Continuous]
    
  - Name: Zeitplan
    Typ: Text
    Beispiel: Täglich 22:00, Wöchentlich Sonntag 02:00
    
  - Name: Retention
    Typ: Text
    Beispiel: 30 Tage lokal, 1 Jahr Cloud
    
  - Name: Quellsysteme
    Typ: Tag (Configurations, Multiple)
    
  - Name: Backup-Ziel
    Typ: Text
    
  - Name: Verschlüsselung
    Typ: Checkbox
    
  - Name: Letzter erfolgreicher Test
    Typ: Date
    
  - Name: Recovery-Anleitung
    Typ: Textbox
    
  - Name: Zugangsdaten
    Typ: Tag (Passwords)
```

## Template-Erstellung Guidelines

### Feldtypen und ihre Verwendung

| Feldtyp | Verwendung | Beispiel |
|---------|------------|----------|
| **Text** | Kurze Eingaben | Name, Version |
| **Textbox** | Längere Beschreibungen | Notizen, Anleitungen |
| **Select** | Vordefinierte Optionen | Status, Typ |
| **Number** | Numerische Werte | Port, VLAN ID |
| **Checkbox** | Ja/Nein-Felder | Aktiv, Verschlüsselt |
| **Date** | Datumswerte | Ablaufdatum, Letzter Test |
| **URL** | Web-Links | Support-URL, Dokumentation |
| **Password** | Sensible Daten | Lizenzschlüssel |
| **Tag** | Verknüpfungen | Server, Kontakte |
| **Upload** | Dateianhänge | Diagramme, Zertifikate |

### Best Practices für Templates

1. **Pflichtfelder sparsam einsetzen**
   - Nur wirklich notwendige Felder als Pflicht markieren
   - Mindestens: Name-Feld

2. **Gruppierung nutzen**
   - Logisch zusammengehörige Felder gruppieren
   - Header/Sections für Übersichtlichkeit

3. **Beschreibungen hinzufügen**
   - Jedes Feld mit Hilfetext versehen
   - Beispiele im Hilfetext angeben

4. **Tags für Beziehungen**
   - Tag-Felder für Verknüpfungen zu anderen Assets
   - Multiple Tags erlauben wo sinnvoll

## Template-Dokumentation

### Template README
Für jedes Template eine Dokumentation erstellen:

```markdown
# [Template Name]

## Zweck
Beschreibung wofür dieses Template verwendet wird.

## Wann verwenden
- Situation 1
- Situation 2

## Pflichtfelder
- Feld 1: Beschreibung
- Feld 2: Beschreibung

## Best Practices
- Empfehlung 1
- Empfehlung 2

## Beispiel
[Ausgefülltes Beispiel]
```

## Import/Export

### Template-Export
- Templates können als JSON exportiert werden
- Über Account → Export Data → Flexible Asset Templates

### Template-Import
- Vorgefertigte Templates aus der ITGlue Template Library
- Eigene Templates als JSON importieren
- Account → Flexible Asset Types → Import
