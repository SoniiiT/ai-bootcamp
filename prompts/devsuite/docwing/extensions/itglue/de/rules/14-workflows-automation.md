# Regel 14: Workflows & Automatisierung

## Zweck
Diese Regel definiert Standards für die Dokumentation von Workflows, Automatisierungen und Benachrichtigungen in ITGlue.

## Flags (Kennzeichnungen)

### Flag-Dokumentation
```markdown
## ITGlue Flags: Übersicht

### Verfügbare Flags
| Flag | Farbe | Verwendung |
|------|-------|------------|
| Needs Review | Orange | Dokument muss geprüft werden |
| Reviewed | Grün | Dokument wurde geprüft |
| Outdated | Rot | Dokument ist veraltet |
| Important | Gelb | Wichtiges Dokument |
| Work in Progress | Blau | Dokument in Bearbeitung |

### Flag-Workflow
1. Dokument erstellen → "Work in Progress"
2. Dokument fertigstellen → "Needs Review"
3. Review durchführen → "Reviewed"
4. Veraltete erkennen → "Outdated"
```

### Flag-Regeln dokumentieren
```markdown
## Flag-Richtlinien

### Zuweisung
| Situation | Flag |
|-----------|------|
| Neues Dokument | Work in Progress |
| Bereit zur Prüfung | Needs Review |
| Nach Review | Reviewed |
| Älter als 90 Tage | Needs Review |
| Nicht mehr aktuell | Outdated |

### Verantwortlichkeiten
| Rolle | Aktionen |
|-------|----------|
| Autor | Flag auf "Needs Review" setzen |
| Reviewer | Flag auf "Reviewed" setzen |
| Admin | Regelmäßige Flag-Reports prüfen |
```

## Webhooks

### Webhook-Dokumentation
```markdown
## ITGlue Webhooks: Konfiguration

### Webhook-Übersicht
| Eigenschaft | Wert |
|-------------|------|
| Endpoint URL | [Webhook-URL] |
| Aktiv | Ja/Nein |
| Events | [Event-Typen] |

### Unterstützte Events
| Event | Beschreibung |
|-------|--------------|
| password.created | Passwort erstellt |
| password.updated | Passwort geändert |
| password.deleted | Passwort gelöscht |
| document.created | Dokument erstellt |
| document.updated | Dokument geändert |
| configuration.created | Konfiguration erstellt |
| organization.created | Organisation erstellt |
```

### Webhook-Payload Beispiel
```markdown
## Webhook Payload Struktur

### Beispiel: password.updated
```json
{
  "event": "password.updated",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "id": 12345,
    "name": "Admin Password",
    "organization_id": 100,
    "resource_type": "Password",
    "updated_by": "user@example.com"
  }
}
```

### Verarbeitung
- Webhook-Empfänger muss HTTP 200 zurückgeben
- Timeout: 30 Sekunden
- Retry bei Fehler: 3 Versuche
```

## E-Mail-Benachrichtigungen

### Benachrichtigungseinstellungen
```markdown
## ITGlue E-Mail-Benachrichtigungen

### Benutzer-Einstellungen
| Benachrichtigung | Standard | Beschreibung |
|-----------------|----------|--------------|
| Password Expiry | An | X Tage vor Ablauf |
| Document Updates | Aus | Bei Änderungen |
| Review Reminders | An | Für zugewiesene Reviews |
| Weekly Digest | Aus | Wöchentliche Zusammenfassung |

### Konfiguration
- Benutzer > Preferences > Notifications
- Administrator kann Defaults setzen
```

### Passwort-Ablauf-Benachrichtigungen
```markdown
## Passwort-Ablauf-Benachrichtigungen

### Zeiträume
| Tage vor Ablauf | Aktion |
|-----------------|--------|
| 30 Tage | Erste Warnung |
| 14 Tage | Erinnerung |
| 7 Tage | Dringende Warnung |
| 0 Tage | Abgelaufen-Meldung |

### Empfänger
- Passwort-Ersteller
- Zugewiesene Benutzer
- Admin (optional)
```

## Slack-Integration

### Slack-Benachrichtigungen dokumentieren
```markdown
## ITGlue Slack-Integration

### Konfiguration
| Eigenschaft | Wert |
|-------------|------|
| Workspace | [Slack Workspace] |
| Channel | [Slack Channel] |
| Bot Name | ITGlue |
| Events | [Konfigurierte Events] |

### Setup
1. Slack App erstellen/installieren
2. Webhook URL generieren
3. In ITGlue unter Admin > Integrations > Slack eintragen
4. Events auswählen

### Unterstützte Events
- Passwort erstellt/geändert
- Dokument erstellt/geändert
- Konfiguration geändert
- Review erforderlich
```

## Microsoft Teams-Integration

### Teams-Benachrichtigungen dokumentieren
```markdown
## ITGlue Microsoft Teams-Integration

### Konfiguration
| Eigenschaft | Wert |
|-------------|------|
| Team | [Teams-Name] |
| Channel | [Channel-Name] |
| Webhook URL | [Incoming Webhook URL] |

### Setup über Workflows
1. Teams > Channel > Workflows
2. "Post to a channel when a webhook request is received"
3. Workflow URL kopieren
4. In ITGlue Webhook konfigurieren

### Alternative: Power Automate
- Webhook empfangen
- Nachricht in Teams posten
- Adaptive Cards für reichere Darstellung
```

## Zapier-Integration

### Zapier-Workflows dokumentieren
```markdown
## ITGlue Zapier-Integration

### Trigger (ITGlue → Andere Apps)
| Trigger | Beschreibung |
|---------|--------------|
| New Password | Neues Passwort erstellt |
| Updated Password | Passwort geändert |
| New Document | Neues Dokument |
| New Configuration | Neue Konfiguration |

### Actions (Andere Apps → ITGlue)
| Action | Beschreibung |
|--------|--------------|
| Create Password | Passwort erstellen |
| Create Document | Dokument erstellen |
| Create Configuration | Konfiguration erstellen |

### Beispiel-Zaps
| Zap | Trigger | Action |
|-----|---------|--------|
| Ticket → ITGlue | New ConnectWise Ticket | Create ITGlue Document |
| Password Alert | ITGlue Password Expiring | Slack Message |
| New Client Setup | New Organization | Create Trello Card |
```

## Automatisierte Workflows

### Workflow-Dokumentationsvorlage
```markdown
## Automatisierter Workflow: [Name]

### Übersicht
| Eigenschaft | Wert |
|-------------|------|
| Trigger | [Auslöser] |
| Ziel | [Zielaktion] |
| Frequenz | [Einmalig/Wiederkehrend] |
| Status | Aktiv/Inaktiv |

### Workflow-Schritte
1. [Trigger-Beschreibung]
2. [Bedingungen prüfen]
3. [Aktion ausführen]
4. [Benachrichtigung senden]

### Beteiligte Systeme
| System | Rolle |
|--------|-------|
| ITGlue | [Trigger/Action] |
| [System 2] | [Rolle] |

### Fehlerbehandlung
- Bei Fehler: [Aktion]
- Eskalation an: [Person/Team]
```

## Review-Workflows

### Review-Prozess dokumentieren
```markdown
## Dokumenten-Review-Workflow

### Ablauf
1. Autor setzt Flag auf "Needs Review"
2. Reviewer erhält Benachrichtigung
3. Reviewer prüft Dokument
4. Reviewer setzt Flag auf "Reviewed"
5. Autor erhält Bestätigung

### Automatische Review-Trigger
| Trigger | Aktion |
|---------|--------|
| 90 Tage ohne Update | Flag auf "Needs Review" |
| Ablaufdatum erreicht | Benachrichtigung an Autor |
| Kritisches Update | Sofortige Review |

### Review-Checkliste
- [ ] Inhalt aktuell?
- [ ] Links funktionieren?
- [ ] Related Items korrekt?
- [ ] Tags passend?
```

## Audit-Trail-Nutzung

### Audit-Trail dokumentieren
```markdown
## ITGlue Audit Trail

### Verfolgbare Aktionen
| Aktion | Details |
|--------|---------|
| Login | Benutzer, IP, Zeitpunkt |
| Password View | Wer hat wann zugegriffen |
| Document Edit | Änderungen, Benutzer |
| Permission Change | Vorher/Nachher |

### Audit-Berichte
- Admin > Reports > Audit Log
- Filter nach Zeitraum, Benutzer, Aktion
- Export als CSV

### Compliance-Nutzung
- Nachweis für Audits
- Zugriffskontrolle validieren
- Änderungen nachvollziehen
```

## Automatisierung Best Practices

### Best Practices
```markdown
## Automatisierung: Best Practices

### Webhook-Sicherheit
- HTTPS-Endpunkte verwenden
- Webhook-Secrets validieren
- Rate Limiting implementieren

### Benachrichtigungs-Hygiene
- Nicht zu viele Benachrichtigungen
- Relevante Events auswählen
- Duplikate vermeiden

### Workflow-Dokumentation
- Jeden Workflow dokumentieren
- Verantwortliche benennen
- Regelmäßig Review

### Monitoring
- Webhook-Fehler überwachen
- Benachrichtigungs-Zustellung prüfen
- Audit-Log regelmäßig prüfen
```

## Checkliste Automatisierung

### Webhooks
- [ ] Webhook-Endpunkt definiert
- [ ] HTTPS aktiviert
- [ ] Events ausgewählt
- [ ] Testdaten gesendet
- [ ] Fehlerbehandlung implementiert

### Benachrichtigungen
- [ ] E-Mail-Benachrichtigungen konfiguriert
- [ ] Slack/Teams integriert (optional)
- [ ] Passwort-Ablauf-Warnungen aktiv
- [ ] Review-Reminder aktiv

### Workflows
- [ ] Workflows dokumentiert
- [ ] Verantwortliche benannt
- [ ] Testlauf durchgeführt
- [ ] Monitoring aktiv
