# Regel 06: Network Glue Dokumentation

## Zweck
Diese Regel definiert Standards für die Dokumentation von Network Glue Features, Netzwerk-Discovery und Geräteverwaltung.

## Network Glue Collector Deployment

### Collector-Dokumentation
```markdown
## Network Glue Collector: [Standort/Kunde]

### Collector-Informationen
| Eigenschaft | Wert |
|-------------|------|
| Collector-Name | [Name] |
| Installationsort | [Server/Workstation] |
| IP-Adresse | [IP] |
| Version | [Version] |
| Letzter Sync | [Datum/Uhrzeit] |
| Status | Aktiv/Inaktiv |

### Netzwerk-Konfiguration
- Domain-basiert: Ja/Nein
- SNMP aktiviert: Ja/Nein
- SNMP-Version: v1/v2c/v3
- WMI aktiviert: Ja/Nein
```

## SNMP-Konfiguration dokumentieren

### Hersteller-spezifische SNMP-Anforderungen
- **Meraki**: Dashboard API
- **Cisco**: SNMP v2c/v3 mit spezifischen Community Strings
- **HP/Aruba**: SNMP v2c Standard
- **Fortinet**: SNMP mit FortiOS-spezifischen OIDs
- **SonicWall**: SNMP aktiviert via Verwaltungsoberfläche
- **Sophos**: SNMP mit XG/UTM-spezifischen Einstellungen

### SNMP-Dokumentationsvorlage
```markdown
## SNMP-Konfiguration: [Gerätename]

### Allgemeine Einstellungen
| Parameter | Wert |
|-----------|------|
| SNMP-Version | v2c / v3 |
| Community String | [gespeichert als Passwort] |
| Port | 161 |

### SNMPv3 (falls verwendet)
| Parameter | Wert |
|-----------|------|
| Benutzername | [Username] |
| Auth Protocol | MD5/SHA |
| Privacy Protocol | DES/AES |
| Security Level | noAuthNoPriv/authNoPriv/authPriv |
```

## Netzwerkdiagramme

### Automatische Topologie-Dokumentation
- Physische Verbindungen zwischen Geräten
- Switch-Port-Mappings
- VLAN-Zuordnungen
- Router-/Gateway-Beziehungen

### Manuelle Ergänzungen
```markdown
## Netzwerk-Topologie: [Standort]

### Diagramm-Informationen
- Generiert am: [Datum]
- Letztes Update: [Datum]
- Geräteanzahl: [Anzahl]

### Besonderheiten
- [Manuelle Anmerkungen zu Verbindungen]
- [Nicht erkannte Geräte]
- [Spezielle Konfigurationen]
```

## Device Matching

### Matching-Logik dokumentieren
1. Primäre MAC-Adresse + Seriennummer (exakt)
2. Nur primäre MAC-Adresse
3. Nur Seriennummer
4. Gerätename (manuelles Matching)

### Dokumentationsformat
```markdown
## Device Matching: [Organisation]

### Matched Devices
| ITGlue Config | Network Glue Device | Match-Methode |
|---------------|---------------------|---------------|
| [Config-Name] | [Device-Name] | MAC/Serial/Name |

### Unmatched Devices
| Device | Grund | Aktion |
|--------|-------|--------|
| [Name] | [Warum nicht gematcht] | Ignorieren/Manuell matchen/Neu erstellen |
```

## Windows Domain Networks

### Active Directory Integration
```markdown
## AD-Integration: [Domain]

### Konfiguration
- Domain Controller: [DC-Name/IP]
- GPO für Collector: Ja/Nein
- WMI aktiviert: Ja/Nein
- Remote Registry: Ja/Nein

### GPO-Einstellungen
- Computerkonfiguration > Administrative Vorlagen > Netzwerk
- WMI-Firewall-Regeln
- Remote Registry Service aktiviert

### Sync-Informationen
- AD-Benutzer: [Anzahl]
- AD-Computer: [Anzahl]
- AD-Gruppen: [Anzahl]
- Letzte Synchronisation: [Datum]
```

## Multiple Subnets

### Subnet-Dokumentation
```markdown
## Netzwerk-Subnets: [Standort]

### Subnet-Übersicht
| Subnet | VLAN | Beschreibung | SNMP-fähig |
|--------|------|--------------|------------|
| 192.168.1.0/24 | 10 | Server | Ja |
| 192.168.2.0/24 | 20 | Clients | Nein |
| 10.0.0.0/24 | 100 | Management | Ja |

### Routing
- Gateway: [IP]
- DNS-Server: [IPs]
```

## Firewall-Regeln für Network Glue

### Erforderliche Ports
```markdown
## Firewall-Regeln: Network Glue

### Ausgehend (Collector → Internet)
| Port | Protokoll | Ziel | Zweck |
|------|-----------|------|-------|
| 443 | TCP | *.itglue.com | API-Kommunikation |

### Intern (Collector → Netzwerk)
| Port | Protokoll | Zweck |
|------|-----------|-------|
| 161 | UDP | SNMP |
| 135 | TCP | WMI/RPC |
| 445 | TCP | SMB |
| 5985/5986 | TCP | WinRM |
```

## Passwort-Rotation (AD)

### Scheduled Password Rotation
```markdown
## Password Rotation: [Domain]

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Rotationsfrequenz | [1-365 Tage] |
| Aktiviert für | On-Prem AD / Entra ID / Beide |
| Bulk-Aktivierung | Ja/Nein |

### Betroffene Accounts
- Service Accounts: [Liste]
- Admin Accounts: [Liste]
- Ausgeschlossen: [Liste]

### Benachrichtigungen
- Erfolg: [Email/Webhook]
- Fehlschlag: [Email/Webhook]
```

## Netzwerkgerät-Typen

### Konfigurationstypen dokumentieren
- Managed Server
- Managed Workstation
- Firewall
- Switch
- Router
- Access Point
- Printer
- Other

### Automatische Erkennung
```markdown
## Auto-Discovery: [Netzwerk]

### Erkannte Gerätetypen
| Typ | Anzahl | Auto-klassifiziert |
|-----|--------|-------------------|
| Server | [X] | Ja/Nein |
| Workstation | [X] | Ja/Nein |
| Netzwerkgerät | [X] | Ja/Nein |
```

## Checkliste Network Glue Dokumentation

- [ ] Collector installiert und dokumentiert
- [ ] SNMP-Credentials gespeichert
- [ ] Netzwerkbereiche definiert
- [ ] Device Matching durchgeführt
- [ ] Topologie-Diagramm geprüft
- [ ] AD-Integration konfiguriert (falls zutreffend)
- [ ] Firewall-Regeln dokumentiert
- [ ] Password Rotation konfiguriert (falls benötigt)
