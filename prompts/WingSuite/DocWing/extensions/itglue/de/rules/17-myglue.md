# Regel 17: MyGlue-Konfiguration

## Zweck
Diese Regel definiert Standards für die Dokumentation von MyGlue, dem Kunden-Portal für ITGlue.

## MyGlue Übersicht

### MyGlue-Grundlagen
```markdown
## MyGlue: Übersicht

### Was ist MyGlue?
Kundenportal für kontrollierten Zugriff auf ITGlue-Inhalte.

### Hauptfunktionen
| Funktion | Beschreibung |
|----------|--------------|
| Passwort-Management | Kunden verwalten eigene Passwörter |
| Dokumentenzugriff | Freigegebene Dokumente lesen |
| Self-Service | Passwörter selbst zurücksetzen |
| Zusammenarbeit | Gemeinsame Dokumentation |

### Voraussetzungen
- MyGlue-Lizenz aktiviert
- Kunden-Benutzer angelegt
- Berechtigungen konfiguriert
```

## Fünf Bereitstellungsszenarien

### Szenario 1: Passwort-Management
```markdown
## MyGlue Szenario 1: Passwort-Management

### Beschreibung
Kunden nutzen MyGlue ausschließlich zur Passwortverwaltung.

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Passwort-Zugriff | ✅ Aktiviert |
| Dokument-Zugriff | ❌ Deaktiviert |
| Konfigurationen | ❌ Deaktiviert |
| Flexible Assets | ❌ Deaktiviert |

### Benutzer-Berechtigungen
| Berechtigung | Wert |
|--------------|------|
| Passwörter anzeigen | ✅ |
| Passwörter erstellen | Optional |
| Passwörter bearbeiten | Nur eigene |
| Passwörter löschen | ❌ |

### Vorteile
- Minimaler Zugriff
- Sichere Passwortverwaltung
- Einfache Einrichtung
```

### Szenario 2: Interne Dokumentation
```markdown
## MyGlue Szenario 2: Interne Dokumentation

### Beschreibung
Kunden-IT-Team nutzt MyGlue für interne Dokumentation.

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Passwort-Zugriff | ✅ Aktiviert |
| Dokument-Zugriff | ✅ Aktiviert |
| Konfigurationen | ✅ Aktiviert |
| Flexible Assets | Optional |

### Dokumenten-Berechtigungen
| Berechtigung | Wert |
|--------------|------|
| Dokumente anzeigen | ✅ |
| Dokumente erstellen | ✅ |
| Dokumente bearbeiten | ✅ |
| Dokumente löschen | Optional |

### Verwendung
- IT-Team erstellt eigene Dokumentation
- MSP kann Inhalte einsehen
- Gemeinsame Wissensbasis
```

### Szenario 3: Stakeholder-Zugriff
```markdown
## MyGlue Szenario 3: Stakeholder-Zugriff

### Beschreibung
Führungskräfte/Stakeholder erhalten Lesezeige auf relevante Informationen.

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Passwort-Zugriff | ✅ Nur Lesen |
| Dokument-Zugriff | ✅ Nur Lesen |
| Konfigurationen | ❌ Deaktiviert |
| Dashboards | ✅ Aktiviert |

### Benutzer-Berechtigungen
| Berechtigung | Wert |
|--------------|------|
| Anzeigen | ✅ |
| Bearbeiten | ❌ |
| Erstellen | ❌ |
| Löschen | ❌ |

### Typische Inhalte
- Übersichtsdokumente
- Netzwerk-Diagramme
- Compliance-Berichte
- Wichtige Passwörter (CEO)
```

### Szenario 4: Collaboration
```markdown
## MyGlue Szenario 4: Collaboration

### Beschreibung
Volle Zusammenarbeit zwischen MSP und Kunde.

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Passwort-Zugriff | ✅ Voll |
| Dokument-Zugriff | ✅ Voll |
| Konfigurationen | ✅ Voll |
| Flexible Assets | ✅ Aktiviert |

### Berechtigungen
| Bereich | Kunde | MSP |
|---------|-------|-----|
| Passwörter | CRUD | CRUD |
| Dokumente | CRUD | CRUD |
| Configs | CR | CRUD |
| Flex Assets | CR | CRUD |

### Best Practices
- Klare Verantwortlichkeiten definieren
- Änderungsverfolgung aktivieren
- Regelmäßige Synchronisation
```

### Szenario 5: Self-Service
```markdown
## MyGlue Szenario 5: Self-Service

### Beschreibung
Endbenutzer können Passwörter selbst verwalten und zurücksetzen.

### Konfiguration
| Einstellung | Wert |
|-------------|------|
| Self-Service | ✅ Aktiviert |
| Passwort-Reset | ✅ Aktiviert |
| MFA | ✅ Erforderlich |
| SSO | Optional |

### Self-Service Funktionen
| Funktion | Beschreibung |
|----------|--------------|
| Passwort-Anzeige | Eigene Passwörter sehen |
| Passwort-Reset | Selbst zurücksetzen |
| OTP-Generierung | Zeitbasierte Codes |
| SafeShare | Sichere Passwort-Teilung |

### Sicherheit
- MFA für alle Benutzer
- Audit-Log für alle Zugriffe
- Begrenzte Passwort-Sichtbarkeit
```

## MyGlue-Branding

### Branding-Dokumentation
```markdown
## MyGlue Branding

### Anpassbare Elemente
| Element | Anpassung |
|---------|-----------|
| Logo | Eigenes Logo hochladen |
| Farben | Primär-/Sekundärfarben |
| Favicon | Custom Icon |
| Login-Seite | Hintergrundbild |

### Branding-Einstellungen
| Eigenschaft | Wert |
|-------------|------|
| Logo URL | [URL] |
| Primärfarbe | [Hex-Code] |
| Sekundärfarbe | [Hex-Code] |
| Custom Domain | [Domain] (optional) |

### Best Practices
- Logo-Größe: max 200x50px
- Kontrastfarben für Lesbarkeit
- Konsistenz mit Unternehmensbranding
```

## MyGlue-SSO

### SSO für MyGlue
```markdown
## MyGlue SSO-Konfiguration

### Unterstützte Provider
| Provider | Konfiguration |
|----------|---------------|
| Azure AD / Entra ID | SAML 2.0 |
| Google Workspace | SAML 2.0 |
| Okta | SAML 2.0 |
| OneLogin | SAML 2.0 |

### SAML-Einstellungen
| Eigenschaft | Wert |
|-------------|------|
| Entity ID | https://myglue.[subdomain].itglue.com/ |
| ACS URL | https://myglue.[subdomain].itglue.com/users/saml/auth |
| Name ID | Email |

### Konfiguration
1. IdP SAML-App erstellen
2. ITGlue MyGlue SSO konfigurieren
3. Test-Login durchführen
4. SSO-Only aktivieren (optional)
```

## MyGlue-Benutzer

### Benutzer-Management
```markdown
## MyGlue Benutzer-Management

### Benutzer-Typen
| Typ | Beschreibung |
|-----|--------------|
| Full Access | Voller Zugriff auf Org |
| Limited | Eingeschränkter Zugriff |
| Read-Only | Nur Leserechte |

### Benutzer erstellen
| Feld | Wert |
|------|------|
| Name | [Vor- und Nachname] |
| Email | [Email-Adresse] |
| Rolle | [Rolle] |
| Organisation | [Zugewiesene Org] |
| MFA | Erforderlich/Optional |

### Einladungsprozess
1. Benutzer in ITGlue anlegen
2. MyGlue-Zugang aktivieren
3. Einladungs-Email wird gesendet
4. Benutzer setzt Passwort
5. MFA-Einrichtung
```

## MyGlue-Sicherheit

### Sicherheits-Dokumentation
```markdown
## MyGlue Sicherheit

### Sicherheitsmaßnahmen
| Maßnahme | Status |
|----------|--------|
| MFA | ✅ Erzwungen |
| SSO | ✅/❌ |
| IP-Beschränkung | Optional |
| Session-Timeout | [Minuten] |

### Zugriffsebenen
| Ebene | Beschreibung |
|-------|--------------|
| Organisation | Zugriff auf Org-Inhalte |
| Ordner | Ordner-basierter Zugriff |
| Asset | Einzelnes Asset |
| Feld | Feld-basierter Zugriff |

### Audit
- Alle Zugriffe werden protokolliert
- Passwort-Views nachverfolgbar
- Änderungen dokumentiert
```

## MyGlue-Dokumentation für Kunden

### Kunden-Anleitung
```markdown
## MyGlue: Anleitung für Endbenutzer

### Erste Schritte
1. Einladungs-Email öffnen
2. "Account aktivieren" klicken
3. Passwort festlegen
4. MFA einrichten (falls erforderlich)

### Passwörter abrufen
1. In MyGlue einloggen
2. "Passwords" klicken
3. Passwort suchen
4. "Anzeigen" klicken
5. Bei Bedarf kopieren

### Passwort erstellen (falls berechtigt)
1. "New Password" klicken
2. Details eingeben
3. Speichern

### SafeShare nutzen
1. Passwort auswählen
2. "Share" klicken
3. Link kopieren
4. An Empfänger senden
```

## MyGlue-Checkliste

### Einrichtung
- [ ] MyGlue-Lizenz aktiviert
- [ ] Branding konfiguriert
- [ ] SSO eingerichtet (optional)
- [ ] MFA-Richtlinie definiert

### Benutzer
- [ ] Benutzer-Typen definiert
- [ ] Berechtigungen konfiguriert
- [ ] Einladungen versendet
- [ ] MFA-Einrichtung abgeschlossen

### Inhalte
- [ ] Sichtbare Inhalte definiert
- [ ] Ordnerstruktur angepasst
- [ ] Passwort-Kategorien konfiguriert
- [ ] Dokumente freigegeben

### Schulung
- [ ] Kunden-Anleitung erstellt
- [ ] Schulung durchgeführt
- [ ] Support-Kontakt kommuniziert
