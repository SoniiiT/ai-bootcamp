# Regel 3 - Schreibrichtlinien

## Sprachliche Grundsätze

### Klarheit
- **Einfache Sprache**: Kurze Sätze, klare Wortwahl
- **Aktive Formulierungen**: "Klicken Sie auf..." statt "Es wird geklickt auf..."
- **Direkte Ansprache**: "Sie können..." oder imperative Form "Führen Sie aus..."
- **Keine Füllwörter**: Vermeiden von "eigentlich", "gewissermaßen", "sozusagen"

### Konsistenz
- **Terminologie**: Einmal gewählte Begriffe durchgängig verwenden
- **Formatierung**: Gleiche Elemente gleich formatieren
- **Struktur**: Ähnliche Inhalte ähnlich aufbauen
- **Stil**: Einheitlicher Ton in der gesamten Dokumentation

### Präzision
- **Genaue Angaben**: Konkrete Zahlen, Pfade, Befehle
- **Vollständige Befehle**: Copy-paste-fähige Kommandos
- **Eindeutige Verweise**: Klare Referenzen auf andere Abschnitte
- **Aktuelle Informationen**: Versionsnummern, Datumsangaben

## Zielgruppengerechte Sprache

### Für Entwickler
- Technische Fachbegriffe ohne Erklärung
- Code-Beispiele in relevanten Sprachen
- Prägnante Erklärungen
- API-Details und Implementierungshinweise

**Beispiel:**
```
Die `getUserById`-Funktion führt einen O(1)-Lookup im Cache durch 
und fällt bei Cache-Miss auf die Datenbank zurück.
```

### Für Endnutzer
- Einfache, nicht-technische Sprache
- Schritt-für-Schritt-Anleitungen
- Screenshots und visuelle Hilfen
- Vermeidung von Fachjargon

**Beispiel:**
```
Klicken Sie auf den Button "Speichern", um Ihre Änderungen zu übernehmen.
Ein grünes Häkchen zeigt an, dass das Speichern erfolgreich war.
```

### Für Administratoren
- Technische Details mit Kontext
- Sicherheitshinweise
- Konfigurationsoptionen vollständig
- Troubleshooting-Informationen

**Beispiel:**
```
Setzen Sie `max_connections` auf einen Wert zwischen 100 und 500.
Bei Werten über 500 kann die Performance beeinträchtigt werden.
Überwachen Sie die aktiven Verbindungen mit `SHOW PROCESSLIST`.
```

## Formatierungsregeln

### Überschriften
```markdown
# Haupttitel (H1) - einmal pro Dokument
## Hauptabschnitt (H2)
### Unterabschnitt (H3)
```

### Hervorhebungen
- **Fett**: Wichtige Begriffe, UI-Elemente
- *Kursiv*: Betonung, neue Begriffe bei Einführung
- `Code`: Befehle, Dateinamen, Code-Elemente
- > Blockzitate: Wichtige Hinweise, Warnungen

### Listen
**Aufzählungen** für:
- Ungeordnete Sammlungen
- Features oder Eigenschaften
- Optionen ohne Reihenfolge

**Nummerierungen** für:
1. Schrittweise Anleitungen
2. Priorisierte Listen
3. Sequentielle Prozesse

### Tabellen
```markdown
| Spalte A | Spalte B | Spalte C |
|----------|----------|----------|
| Wert 1   | Wert 2   | Wert 3   |
```

**Verwenden für:**
- Parameter-Referenzen
- Vergleiche
- Strukturierte Daten

### Code-Blöcke
````markdown
```sprache
// Code mit Syntax-Highlighting
```
````

**Sprachen immer angeben:**
- `bash`, `powershell` für Befehle
- `json`, `yaml` für Konfiguration
- `python`, `javascript`, etc. für Code

### Hinweisboxen

**Info:**
> 💡 **Hinweis:** Zusätzliche hilfreiche Information.

**Warnung:**
> ⚠️ **Warnung:** Wichtiger Sicherheitshinweis oder Einschränkung.

**Fehler:**
> ❌ **Achtung:** Kritischer Hinweis, der beachtet werden muss.

**Erfolg:**
> ✅ **Tipp:** Best Practice oder Empfehlung.

## Qualitätskriterien

### Checkliste vor Veröffentlichung
- [ ] Rechtschreibung geprüft
- [ ] Alle Links funktionieren
- [ ] Code-Beispiele getestet
- [ ] Screenshots aktuell
- [ ] Konsistente Terminologie
- [ ] Zielgruppe berücksichtigt
- [ ] Struktur logisch
- [ ] Versionsinformationen korrekt
