# Regel 4 - Dokumentationsvorlagen

## Zweck
Diese Regel stellt wiederverwendbare Vorlagen für verschiedene Dokumentationstypen bereit.

## Vorlagen

### README.md (Projekt)

```markdown
# Projektname

Kurze Beschreibung des Projekts in 1-2 Sätzen.

## Funktionen

- Feature 1
- Feature 2
- Feature 3

## Voraussetzungen

- Voraussetzung 1
- Voraussetzung 2

## Installation

```bash
# Installationsbefehl
```

## Verwendung

```bash
# Beispielbefehl
```

## Konfiguration

Beschreibung der Konfigurationsoptionen.

## Beitragen

Hinweise für Contributor.

## Lizenz

[Lizenztyp](LICENSE)
```

---

### API-Endpunkt

```markdown
## [HTTP-Methode] /pfad/zum/endpunkt

Kurze Beschreibung des Endpunkts.

### Authentifizierung

Erforderliche Authentifizierung beschreiben.

### Parameter

#### Path-Parameter

| Name | Typ | Beschreibung |
|------|-----|--------------|
| id   | string | Ressourcen-ID |

#### Query-Parameter

| Name | Typ | Erforderlich | Standard | Beschreibung |
|------|-----|--------------|----------|--------------|
| limit | integer | Nein | 10 | Maximale Anzahl |

#### Request-Body

```json
{
  "field": "value"
}
```

### Response

#### Erfolg (200)

```json
{
  "id": "123",
  "name": "Beispiel"
}
```

#### Fehler

| Code | Beschreibung |
|------|--------------|
| 400  | Ungültige Anfrage |
| 404  | Nicht gefunden |
| 500  | Serverfehler |

### Beispiel

```bash
curl -X GET "https://api.example.com/pfad" \
  -H "Authorization: Bearer TOKEN"
```
```

---

### Funktion/Methode

```markdown
## funktionsName

Kurze Beschreibung der Funktion.

### Signatur

```typescript
function funktionsName(param1: Typ1, param2?: Typ2): RückgabeTyp
```

### Parameter

| Name | Typ | Erforderlich | Beschreibung |
|------|-----|--------------|--------------|
| param1 | Typ1 | Ja | Beschreibung |
| param2 | Typ2 | Nein | Beschreibung |

### Rückgabe

- **Typ:** `RückgabeTyp`
- **Beschreibung:** Was wird zurückgegeben?

### Exceptions

| Exception | Bedingung |
|-----------|-----------|
| TypeError | Wenn param1 ungültig |

### Beispiele

```typescript
// Einfaches Beispiel
const result = funktionsName("wert");

// Komplexes Beispiel
const result = funktionsName("wert", { option: true });
```

### Siehe auch

- [Verwandte Funktion](#)
```

---

### Konfigurationsoption

```markdown
### optionName

Kurze Beschreibung der Option.

- **Typ:** `string | number | boolean`
- **Standard:** `"standardwert"`
- **Erforderlich:** Nein
- **Umgebungsvariable:** `OPTION_NAME`

#### Beschreibung

Ausführliche Beschreibung der Option und ihrer Auswirkungen.

#### Werte

| Wert | Beschreibung |
|------|--------------|
| "a" | Bedeutung von A |
| "b" | Bedeutung von B |

#### Beispiel

```yaml
config:
  optionName: "wert"
```

#### Hinweise

> ⚠️ **Warnung:** Besondere Hinweise zur Verwendung.
```

---

### Changelog-Eintrag

```markdown
## [Version] - YYYY-MM-DD

### Hinzugefügt
- Neue Funktion X (#123)
- Unterstützung für Y

### Geändert
- Verbessertes Verhalten von Z
- Aktualisierte Abhängigkeiten

### Behoben
- Fix für Problem A (#456)
- Korrektur in Modul B

### Entfernt
- Veraltete Funktion C

### Sicherheit
- Behebung der Sicherheitslücke CVE-XXXX-XXXX

### Veraltet
- Funktion D wird in v2.0 entfernt
```

---

### Troubleshooting-Eintrag

```markdown
### Problem: [Kurze Beschreibung]

#### Symptom

Was sieht der Nutzer? Welche Fehlermeldung erscheint?

```
Fehlermeldung hier
```

#### Ursache

Warum tritt dieses Problem auf?

#### Lösung

1. Erster Schritt
2. Zweiter Schritt
3. Verifizierung

```bash
# Lösungsbefehl
```

#### Prävention

Wie kann man das Problem in Zukunft vermeiden?
```

## Verwendungshinweise

1. Vorlage kopieren und anpassen
2. Platzhalter durch echte Inhalte ersetzen
3. Nicht benötigte Abschnitte entfernen
4. Konsistenz mit bestehender Dokumentation wahren
