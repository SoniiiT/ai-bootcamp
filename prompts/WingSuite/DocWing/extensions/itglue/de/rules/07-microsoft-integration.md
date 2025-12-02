# Regel 07: Microsoft Integration

## Zweck
Diese Regel definiert Dokumentationsstandards für die Microsoft/Azure-Integration in ITGlue, einschließlich Intune, Entra ID und GDAP.

## Integration-Setup dokumentieren

### Azure App Registration
```markdown
## Microsoft Integration: [Tenant-Name]

### App Registration
| Eigenschaft | Wert |
|-------------|------|
| Application (Client) ID | [GUID] |
| Directory (Tenant) ID | [GUID] |
| Client Secret | [gespeichert als ITGlue Passwort] |
| Ablaufdatum Secret | [Datum] |

### Redirect URI
- NA: https://[subdomain].itglue.com/microsofts
- EU: https://[subdomain].eu.itglue.com/microsofts
- AU: https://[subdomain].au.itglue.com/microsofts
```

## API-Berechtigungen

### Erforderliche Permissions dokumentieren
```markdown
## API Permissions: Microsoft Graph

### Delegated Permissions
| Permission | Typ | Status |
|------------|-----|--------|
| User.Read.All | Delegated | ✓ Granted |
| Device.Read.All | Delegated | ✓ Granted |
| Directory.Read.All | Delegated | ✓ Granted |
| BitLockerKey.Read.All | Delegated | ✓ Granted |

### Application Permissions
| Permission | Typ | Status |
|------------|-----|--------|
| User.Read.All | Application | ✓ Granted |
| Device.Read.All | Application | ✓ Granted |

### Admin Consent
- Erteilt von: [Admin-Name]
- Datum: [Datum]
```

## GDAP-Konfiguration

### Granular Delegated Admin Privileges
```markdown
## GDAP Setup: [Partner Tenant]

### Service Account
| Eigenschaft | Wert |
|-------------|------|
| Benutzername | [UPN] |
| Zugewiesen zu | AdminAgents Gruppe |
| Rolle | Global Administrator |
| MFA aktiviert | Ja (erforderlich) |

### Security Group
| Eigenschaft | Wert |
|-------------|------|
| Gruppenname | [Name] |
| Gruppentyp | Security |
| Mitglieder | Service Account |

### GDAP Beziehungen
| Kunde/Tenant | Zugewiesene Rolle | Status |
|--------------|-------------------|--------|
| [Tenant A] | Global Administrator | Aktiv |
| [Tenant B] | Privileged Role Admin | Aktiv |
```

## Field Mappings

### Microsoft → ITGlue Mappings
```markdown
## Field Mappings: Microsoft Integration

### Organisationen
| Microsoft Feld | ITGlue Feld |
|----------------|-------------|
| Tenant Name | Organization Name |
| Address | Location Address |
| Phone | Location Phone |

### Kontakte
| Microsoft Feld | ITGlue Feld |
|----------------|-------------|
| First Name + Last Name | Name |
| Job Title | Title |
| Phone | Office Phone |
| Mobile | Mobile Phone |
| User Name | Primary Email |
| Alias | Mailbox Aliases |

### Konfigurationen (Intune)
| Microsoft Feld | ITGlue Feld |
|----------------|-------------|
| Device Type | Configuration Type |
| Device Name | Hostname |
| Serial Number | Serial Number |
| Manufacturer | Manufacturer |
| Model | Model |
```

## Intune Device Sync

### Device-Dokumentation
```markdown
## Intune Devices: [Organisation]

### Sync-Einstellungen
- Sync aktiviert: Ja/Nein
- Letzte Synchronisation: [Datum]
- Device-Anzahl: [Anzahl]

### Device Matching
| Matching-Regel | Priorität |
|----------------|-----------|
| MAC + Serial | 1 (höchste) |
| MAC only | 2 |
| Serial only | 3 |
| Device Name | 4 (manuell) |

### Copilot Smart Relate
- Automatische Verknüpfung: Intune Device ↔ M365 User
```

## BitLocker Recovery Keys

### BitLocker-Dokumentation
```markdown
## BitLocker Keys: [Organisation]

### Voraussetzungen
- Network Glue Subscription: Erforderlich
- API Permission: BitLockerKey.Read.All (Delegated)
- Intune Device Sync: Aktiviert

### Ordnerstruktur
- Ordner: "BitLocker Keys (via Network Glue)"
- Kategorie: BitLocker Keys (via Network Glue)
- Typ: Automatisch generiert

### Key-Informationen
| Feld | Beschreibung |
|------|--------------|
| Name | Device Name aus Intune |
| Username | BitLocker Recovery Key ID |
| Password | 48-stelliger Recovery Key |
| Notes | Automatisch befüllt |

### Einschränkungen
- Keys können nicht bearbeitet werden
- Archivierte Keys für ältere Versionen
- Kann nicht außerhalb des Ordners verschoben werden
- Password Rotation nicht verfügbar
```

## Microsoft 365 Lizenzen

### Lizenz-Sync dokumentieren
```markdown
## M365 Licenses: [Organisation]

### Sync-Konfiguration
- Lizenztypen synchronisieren: Ja/Nein
- Zugewiesene Lizenzen anzeigen: Ja/Nein

### Lizenzübersicht
| Lizenztyp | Gesamt | Zugewiesen | Verfügbar |
|-----------|--------|------------|-----------|
| M365 Business Basic | [X] | [Y] | [Z] |
| M365 Business Standard | [X] | [Y] | [Z] |
```

## Entra ID Sync

### Sync-Einstellungen
```markdown
## Entra ID Sync: [Tenant]

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Sync aktiviert | Ja/Nein |
| BitLocker Keys | Ja/Nein |
| Lizenzinformationen | Ja/Nein |

### Kontakt-Sync
- Alle Benutzer: Ja/Nein
- Gefilterte Benutzer: [Filterkriterien]
- Letzte Synchronisation: [Datum]
```

## Matching-Logik

### Organisations-Matching
```markdown
## Matching: Microsoft Tenants

### Automatisches Matching
- Kriterium: Exakter Tenant Name ↔ Organization Name

### Manuelles Matching erforderlich bei
- Abweichende Namen
- Neue Tenants ohne ITGlue-Gegenstück
- Multi-Tenant-Szenarien
```

## Checkliste Microsoft Integration

- [ ] Azure App Registration erstellt
- [ ] API Permissions konfiguriert
- [ ] Admin Consent erteilt
- [ ] GDAP eingerichtet (falls Partner)
- [ ] Service Account mit MFA erstellt
- [ ] Security Group konfiguriert
- [ ] Tenant-Matching durchgeführt
- [ ] Intune Device Sync aktiviert
- [ ] BitLocker Key Sync aktiviert
- [ ] Contact Sync konfiguriert
- [ ] Client Secret Ablauf dokumentiert
