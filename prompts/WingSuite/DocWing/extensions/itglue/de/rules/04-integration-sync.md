# Regel 4 - Integration und Synchronisation

## Übersicht

ITGlue unterstützt zahlreiche Integrationen für automatische Datensynchronisation. Diese Regel beschreibt Best Practices für die Dokumentation von Integrationsdaten.

## PSA-Integration

### Synchronisierte Datentypen

| Datentyp | Richtung | Beschreibung |
|----------|----------|--------------|
| Organizations | PSA → ITGlue | Kundenorganisationen |
| Configurations | Bidirektional | Geräte/Assets |
| Contacts | Bidirektional | Ansprechpartner |
| Locations | PSA → ITGlue | Standorte |
| Tickets | PSA → ITGlue | Service-Tickets |

### Dokumentations-Best-Practices

**Sync-Status dokumentieren:**
```markdown
## Integration Status

| System | Status | Letzter Sync | Anmerkungen |
|--------|--------|--------------|-------------|
| Autotask | ✅ Aktiv | [Datum/Zeit] | Bidirektional |
| Datto RMM | ✅ Aktiv | [Datum/Zeit] | Device Sync |
| Microsoft 365 | ✅ Aktiv | [Datum/Zeit] | User/License Sync |
```

**Sync-Konfiguration dokumentieren:**
```markdown
## Autotask Sync-Konfiguration

### Account Types (synchronisiert)
- Customer
- Partner

### Configuration Item Types
- Server
- Workstation
- Network Device
- Printer

### Nicht synchronisiert
- Internal (Interne Testorganisationen)
- Prospect (Interessenten)
```

## RMM-Integration

### Datto RMM Dokumentation

**Matching-Dokumentation:**
```markdown
## Datto RMM Site Matching

| ITGlue Organization | Datto RMM Site | Status |
|---------------------|----------------|--------|
| Kunde A GmbH | Kunde_A | ✅ Matched |
| Kunde B AG | KundeB-Site | ✅ Matched |
| Kunde C | [Kein Match] | ⚠️ Unmatched |
```

**Device Sync Status:**
```markdown
## Configuration Sync

### Automatisch synchronisierte Felder
- Name
- Serial Number
- Manufacturer
- Model
- Operating System
- IP Address
- MAC Address

### Manuell zu pflegende Felder
- Asset Tag
- Assigned Contact
- Location
- Notes
- Related Items
```

## Microsoft Integration

### Microsoft 365 / GDAP

**Tenant-Dokumentation:**
```markdown
## Microsoft 365 Tenant

### Tenant Information
- **Tenant Name:** [Name]
- **Tenant ID:** [GUID]
- **GDAP Status:** ✅ Aktiv
- **Letzter Sync:** [Datum/Zeit]

### Synchronisierte Daten
- [ ] Users → Contacts
- [ ] Licenses → Flexible Assets
- [ ] Groups → Flexible Assets

### Berechtigungen
- Directory Reader
- Security Reader
- License Administrator
```

## Network Glue

### Setup-Dokumentation

```markdown
## Network Glue Konfiguration

### Collector Information
- **Collector Name:** [Name]
- **Version:** [X.X.X]
- **Installiert auf:** [Server/Device]
- **Status:** ✅ Online

### Scan-Konfiguration
- **IP-Bereiche:** [Subnet-Liste]
- **Scan-Frequenz:** [Stündlich/Täglich]
- **SNMP Version:** [v2c/v3]
- **AD Integration:** [Ja/Nein]

### Credentials
- **SNMP Community:** [Referenz zu Passwort]
- **AD Service Account:** [Referenz zu Passwort]
```

### Matching-Regeln

```markdown
## Network Glue Matching Rules

### Automatisches Matching
Geräte werden automatisch gematched basierend auf:
1. Serial Number
2. MAC Address
3. Hostname

### Manuelles Matching erforderlich
- Virtuelle Maschinen ohne eindeutige Identifier
- Netzwerkgeräte ohne Serial Number
- Drucker ohne MAC Address
```

## Sync-Fehler dokumentieren

### Fehlerprotokoll-Template

```markdown
## Integration Sync Log

### [Datum] - [Integration Name]

#### Fehler
| Zeit | Fehlertyp | Betroffenes Objekt | Aktion |
|------|-----------|-------------------|--------|
| [Zeit] | Matching Failed | [Objekt] | Manuell matchen |
| [Zeit] | Sync Error | [Objekt] | Support kontaktiert |

#### Gelöste Probleme
- [Beschreibung des Problems und der Lösung]

#### Offene Punkte
- [ ] [Offener Punkt 1]
- [ ] [Offener Punkt 2]
```

## Zwei-Wege-Sync Regeln

### Source of Truth definieren

```markdown
## Data Ownership

| Datenfeld | Source of Truth | Bemerkung |
|-----------|-----------------|-----------|
| Organization Name | PSA | PSA gewinnt bei Konflikt |
| Contact Info | PSA | Automatisch überschrieben |
| Passwords | ITGlue | Nur in ITGlue verwaltet |
| Notes | ITGlue | Nicht synchronisiert |
| Asset Tag | ITGlue | Manuell in ITGlue gepflegt |
```

### Konfliktlösung

```markdown
## Sync-Konfliktlösung

### Regel 1: PSA gewinnt für Basisdaten
- Organization Name
- Contact First/Last Name
- Configuration Serial Number

### Regel 2: ITGlue gewinnt für Dokumentation
- Notes/Bemerkungen
- Related Items
- Embedded Passwords
- Documents
```
