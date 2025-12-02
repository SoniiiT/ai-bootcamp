# Regel 05: Sicherheit und Zugriffskontrolle

## Zweck
Diese Regel definiert die Dokumentationsstandards für ITGlue Sicherheitsfeatures, Berechtigungen und Zugriffskontrollen.

## Benutzerrollen dokumentieren

### Rollenhierarchie
- **Administrator**: Vollzugriff auf alle Einstellungen und Daten
- **Manager**: Administrative Aufgaben ohne destruktive Aktionen
- **Editor**: Erstellen, Bearbeiten, Löschen mit Asset-Berechtigungen
- **Creator**: Erstellen und Bearbeiten ohne Löschrechte
- **Read-only**: Nur Lesezugriff
- **Lite**: Lesezugriff auf maximal 5 Organisationen (kostenlos)

### Dokumentationsformat
```markdown
## Benutzerrolle: [Rollenname]

### Berechtigungen
- Kann Organisationen erstellen: Ja/Nein
- Kann Benutzer verwalten: Ja/Nein
- Kann API-Keys generieren: Ja/Nein
- Kann Daten exportieren: Ja/Nein
- Vault-Zugriff: Ja/Nein

### Einschränkungen
- [Spezifische Limitierungen]

### Anwendungsfälle
- [Typische Nutzungsszenarien]
```

## Gruppen-Dokumentation

### Pflichtfelder
- Gruppenname
- Beschreibung/Zweck
- Zugewiesene Organisationen
- Mitglieder
- Sidebar-Konfiguration (falls angepasst)

### Dokumentationsstruktur
```markdown
## Gruppe: [Gruppenname]

### Übersicht
| Eigenschaft | Wert |
|-------------|------|
| Erstellungsdatum | [Datum] |
| Verantwortlicher | [Person] |
| Mitgliederanzahl | [Anzahl] |

### Organisationszugriff
- [x] Organisation A (vollständig)
- [x] Organisation B (vollständig)
- [ ] Organisation C (kein Zugriff)

### Sidebar-Anpassungen
- Sichtbare Bereiche: [Liste]
- Ausgeblendete Bereiche: [Liste]
```

## Multi-Faktor-Authentifizierung (MFA)

### MFA-Enforcement dokumentieren
- Erzwungene MFA für alle Benutzer
- MFA-Ausnahmeliste
- Recovery-Code-Verfahren
- Authenticator-App-Anforderungen

### Dokumentationsvorlage
```markdown
## MFA-Konfiguration

### Enforcement-Status
- [x] MFA für alle Benutzer erzwungen
- Ausgenommene Benutzer: [Liste oder "Keine"]

### Wiederherstellungsverfahren
1. Recovery-Code verwenden (einmalig)
2. Administrator kontaktieren für MFA-Reset
3. Bei einzigem Administrator: Kaseya Support kontaktieren

### Authenticator-Apps
- Empfohlen: Google Authenticator, Microsoft Authenticator
- Zeitsynchronisation erforderlich
```

## Vault-Sicherheit

### Vault-Dokumentation
- Passphrase-Anforderungen
- Benutzer mit Vault-Zugriff
- Vaulted Passwords vs. normale Passwörter
- Zugriffsentzugsverfahren

### Wichtige Hinweise
- Benutzer mit Vault-Zugriff müssen vor Löschung Zugriff entzogen bekommen
- Vaulted Passwords können NICHT über SafeShare geteilt werden
- Benutzer-spezifische Passphrases erforderlich

## IP-Zugriffskontrollen

### Dokumentationsformat
```markdown
## IP Access Control

### Konfiguration
- Status: Aktiviert/Deaktiviert
- Modus: Alle IPs erlauben / Spezifische IPs erlauben

### Erlaubte IP-Adressen
| IP/Bereich | Beschreibung | Hinzugefügt am |
|------------|--------------|----------------|
| 192.168.1.0/24 | Büro-Netzwerk | [Datum] |
| 10.0.0.5 | VPN-Gateway | [Datum] |

### Limits
- Maximum: 200 einzelne IPs oder Bereiche
- Format: Einzelne IPs oder CIDR-Notation
```

## Asset-Level-Berechtigungen

### Dokumentieren von Asset-Sicherheit
- Wer kann das Asset sehen?
- Wer kann das Asset bearbeiten?
- Gruppenbasierte vs. individuelle Berechtigungen

### Beispielformat
```markdown
## Asset-Berechtigung: [Asset-Name]

### Sichtbarkeit
- Gruppen: [Gruppenliste]
- Einzelbenutzer: [Benutzerliste]

### Bearbeitungsrechte
- Gruppen: [Gruppenliste]
- Einzelbenutzer: [Benutzerliste]

### Vererbung
- Erbt von: [Parent-Asset/Organisation]
```

## SSO-Exemption

### Dokumentation von SSO-Ausnahmen
```markdown
## Enforced SSO Override

### Benutzer mit direkter Authentifizierung
| Benutzer | Grund | Genehmigt von | Datum |
|----------|-------|---------------|-------|
| [Email] | [Begründung] | [Admin] | [Datum] |

### Auswirkungen
- Diese Benutzer können sich NUR mit direkter Authentifizierung anmelden
- SSO ist für diese Benutzer nicht verfügbar
```

## Checkliste für Sicherheitsdokumentation

- [ ] Alle Benutzerrollen dokumentiert
- [ ] Gruppenzuweisungen aktuell
- [ ] MFA-Status für alle Benutzer geprüft
- [ ] Vault-Zugriffsrechte dokumentiert
- [ ] IP-Whitelist aktualisiert
- [ ] Asset-Berechtigungen überprüft
- [ ] SSO-Konfiguration dokumentiert
- [ ] Recovery-Verfahren dokumentiert
