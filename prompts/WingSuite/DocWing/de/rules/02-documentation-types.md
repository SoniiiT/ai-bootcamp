# Regel 2 - Dokumentationstypen und Strukturen

## Dokumentationstypen

### 1. API-Dokumentation
**Zweck:** Entwickler verstehen und nutzen APIs

**Struktur:**
```markdown
# API-Name

## Übersicht
Kurze Beschreibung der API

## Authentifizierung
Wie authentifiziert man sich?

## Endpunkte

### GET /resource
Beschreibung

**Parameter:**
| Name | Typ | Erforderlich | Beschreibung |
|------|-----|--------------|--------------|

**Response:**
```json
{ "example": "response" }
```

**Fehler:**
| Code | Beschreibung |
|------|--------------|
```

**Best Practices:**
- Alle Endpunkte mit Beispielen
- Request/Response-Beispiele als JSON
- Fehlercode mit Beschreibung
- Authentifizierungshinweise

---

### 2. Benutzerhandbuch
**Zweck:** Endnutzer bei der Produktnutzung unterstützen

**Struktur:**
```markdown
# Produktname - Benutzerhandbuch

## Einführung
Was ist das Produkt? Für wen ist es?

## Erste Schritte
1. Installation/Registrierung
2. Erste Konfiguration
3. Erste Nutzung

## Funktionen

### Funktion A
Schritt-für-Schritt-Anleitung

## Häufige Fragen (FAQ)

## Fehlerbehebung
```

**Best Practices:**
- Einfache Sprache verwenden
- Screenshots wo hilfreich
- Nummerierte Schritte
- FAQs für häufige Probleme

---

### 3. Referenzdokumentation
**Zweck:** Schnelles Nachschlagen von Details

**Struktur:**
```markdown
# Konfigurationsreferenz

## Übersicht
Wo wird konfiguriert? Format?

## Optionen

### option_name
- **Typ:** string
- **Standard:** "default"
- **Erforderlich:** Nein
- **Beschreibung:** Was macht diese Option?
- **Beispiel:** `option_name: "wert"`
```

**Best Practices:**
- Alphabetisch oder thematisch geordnet
- Alle Optionen mit Typ und Standard
- Beispiele für komplexe Optionen
- Suchbar strukturieren

---

### 4. Konzeptdokumentation
**Zweck:** Verständnis von Architektur und Design

**Struktur:**
```markdown
# Architektur-Übersicht

## Einführung
Warum diese Architektur?

## Komponenten
Beschreibung der Hauptkomponenten

## Datenfluss
Wie fließen Daten durch das System?

## Designentscheidungen
Warum wurden bestimmte Entscheidungen getroffen?

## Diagramme
Architektur- und Sequenzdiagramme
```

**Best Practices:**
- Diagramme für visuelle Übersicht
- Entscheidungen dokumentieren (ADRs)
- Zielgruppe beachten
- Regelmäßig aktualisieren

---

### 5. Runbook
**Zweck:** Betriebsanleitungen für wiederkehrende Aufgaben

**Struktur:**
```markdown
# Runbook: [Aufgabenname]

## Übersicht
- **Zweck:** Was wird erreicht?
- **Häufigkeit:** Wie oft?
- **Dauer:** Geschätzte Zeit
- **Voraussetzungen:** Was wird benötigt?

## Prozedur

### Schritt 1: [Titel]
```bash
# Befehl
```
Erwartetes Ergebnis: ...

### Schritt 2: [Titel]
...

## Verifizierung
Wie prüft man den Erfolg?

## Rollback
Bei Problemen: Wie macht man rückgängig?

## Troubleshooting
Häufige Probleme und Lösungen
```

**Best Practices:**
- Jeden Schritt einzeln und testbar
- Befehle copy-paste-fähig
- Rollback-Plan obligatorisch
- Regelmäßig testen und aktualisieren

## Strukturprinzipien

### Hierarchie
- Maximal 3 Überschriftenebenen
- Logische Gruppierung
- Konsistente Benennung

### Navigation
- Inhaltsverzeichnis bei langen Dokumenten
- Querverweise zwischen verwandten Themen
- Breadcrumbs bei tiefer Struktur

### Modularität
- Ein Thema pro Dokument
- Wiederverwendbare Abschnitte
- Verlinkung statt Duplizierung
