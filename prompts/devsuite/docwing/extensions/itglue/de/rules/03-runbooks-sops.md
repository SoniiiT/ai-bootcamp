# Regel 3 - Runbooks und SOPs

## Übersicht

Runbooks und SOPs (Standard Operating Procedures) sind strukturierte Dokumentationen für wiederkehrende Prozesse und Arbeitsabläufe in ITGlue.

## ITGlue Runbooks

### Runbook-Typen

**Manuelle Runbooks:**
- Checklisten für manuelle Durchführung
- Schritt-für-Schritt-Anleitungen
- Dokumentation während der Ausführung

**Automatisierte Runbooks:**
- Geplante Ausführung
- Export-Funktionen
- Automatische Berichterstellung

### Runbook-Struktur

```markdown
# Runbook: [Prozessname]

## Metadaten
- **Erstellt:** [Datum]
- **Autor:** [Name]
- **Version:** [X.X]
- **Frequenz:** [Täglich/Wöchentlich/Monatlich/Bei Bedarf]
- **Geschätzte Dauer:** [X Minuten/Stunden]

## Voraussetzungen
- [ ] Zugriff auf System X
- [ ] Berechtigung Y
- [ ] Tool Z installiert

## Checkliste

### Phase 1: Vorbereitung
- [ ] **Schritt 1.1:** Beschreibung
  - Detail A
  - Detail B
- [ ] **Schritt 1.2:** Beschreibung

### Phase 2: Durchführung
- [ ] **Schritt 2.1:** Beschreibung
- [ ] **Schritt 2.2:** Beschreibung

### Phase 3: Nachbereitung
- [ ] **Schritt 3.1:** Dokumentation aktualisieren
- [ ] **Schritt 3.2:** Stakeholder informieren

## Eskalation
Bei Problemen: [Eskalationspfad]

## Abschluss
- [ ] Alle Schritte abgeschlossen
- [ ] Dokumentation aktualisiert
- [ ] Runbook signiert
```

## Standard Operating Procedures (SOPs)

### SOP-Vorlage

```markdown
# SOP: [Prozessname]

## Dokumenteninformationen

| Attribut | Wert |
|----------|------|
| SOP-Nummer | SOP-[XXX] |
| Version | [X.X] |
| Gültig ab | [Datum] |
| Nächste Überprüfung | [Datum] |
| Verantwortlich | [Name/Rolle] |
| Genehmigt von | [Name/Rolle] |

---

## 1. Zweck
Beschreibung des Zwecks dieser SOP.

## 2. Geltungsbereich
Für wen und welche Situationen gilt diese SOP.

## 3. Definitionen
| Begriff | Definition |
|---------|------------|
| Begriff A | Erklärung |
| Begriff B | Erklärung |

## 4. Voraussetzungen
- Berechtigung/Rolle erforderlich
- Benötigte Tools/Systeme
- Vorbereitende Schritte

## 5. Prozedur

### 5.1 [Schritt-Kategorie 1]

#### 5.1.1 [Unterschritt]
1. Aktion durchführen
2. Ergebnis überprüfen
3. Bei Erfolg → weiter zu 5.1.2
4. Bei Fehler → siehe Kapitel 7

### 5.2 [Schritt-Kategorie 2]
...

## 6. Ergebnisdokumentation
- Was muss dokumentiert werden
- Wo wird dokumentiert
- Aufbewahrungsfristen

## 7. Fehlerbehebung

### Problem 1: [Beschreibung]
**Symptom:** 
**Ursache:** 
**Lösung:** 

### Problem 2: [Beschreibung]
...

## 8. Referenzen
- Verwandte SOPs
- Externe Dokumentation
- Kontakte

## 9. Änderungshistorie

| Version | Datum | Autor | Änderung |
|---------|-------|-------|----------|
| 1.0 | [Datum] | [Name] | Erstversion |
```

## Spezifische SOP-Typen

### Client Onboarding SOP

```markdown
# SOP: Client Onboarding

## Checkliste

### Vorbereitung
- [ ] Kundendaten erfassen
- [ ] Organisation in ITGlue anlegen
- [ ] Kontakte importieren

### Dokumentation
- [ ] Netzwerkdokumentation erstellen
- [ ] Passwörter dokumentieren
- [ ] Konfigurationen erfassen

### Integration
- [ ] PSA-Sync aktivieren
- [ ] RMM-Matching durchführen
- [ ] Network Glue einrichten

### Abschluss
- [ ] Dokumentation überprüfen
- [ ] Kunde informieren
- [ ] Team-Übergabe
```

### Client Offboarding SOP

```markdown
# SOP: Client Offboarding

## Checkliste

### Vorbereitung
- [ ] Offboarding-Termin festlegen
- [ ] Verantwortlichkeiten klären

### Datensicherung
- [ ] Account-Export erstellen
- [ ] Wichtige Dokumente sichern
- [ ] Passwort-Export erstellen

### Übergabe
- [ ] Dokumentation an Kunden übergeben
- [ ] Zugangsänderungen dokumentieren

### Bereinigung
- [ ] Organisation archivieren/löschen
- [ ] Berechtigungen entfernen
- [ ] Integration deaktivieren
```

## Cooper Copilot SOP Generator

### Nutzung des SOP Generators
ITGlue bietet mit Cooper Copilot einen intelligenten SOP-Generator:

1. **Chrome-Extension aktivieren**
2. **Aufnahme starten** beim Arbeiten im Browser
3. **Schritte werden automatisch erfasst**
4. **SOP in ITGlue generieren** lassen

### Best Practices
- Screenshots automatisch einbinden lassen
- Sensible Daten vor Export ausblenden
- Generierte SOP nachbearbeiten
- In passenden Ordner ablegen
