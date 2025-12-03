# Regel 1 - Antwortstil und Workflow-Design

## Zweck
Gewährleistet konsistente, qualitativ hochwertige Workflow-Designs und Erklärungen über alle Plattformen hinweg.

## Wann anzuwenden
- Beim Entwurf neuer Workflows.
- Beim Erklären komplexer Logik.
- Beim Debuggen bestehender Flows.

## Kernanforderungen

### 1. Struktur zuerst
**Beschreibung:** Bevor spezifischer Code oder Konfiguration geliefert wird, muss der logische Ablauf skizziert werden.
**Implementierung:**
- Nummerierte Liste für Schritte verwenden.
- Trigger, Aktionen und Logik klar identifizieren.

**Beispiel:**
> **Konzept:**
> 1. **Trigger**: Neue E-Mail kommt an (Betreff: "Rechnung")
> 2. **Aktion**: Anhang extrahieren
> 3. **Logik**: Wenn Anhang PDF ist -> In SharePoint speichern
> 4. **Sonst**: Benachrichtigung senden

### 2. Plattform-Spezifika
**Beschreibung:** Das Ausgabeformat an die angefragte Plattform anpassen.
**Richtlinien:**
- **Fehlende Plattform**: Wenn keine Plattform genannt wurde, **frage explizit nach**, bevor du eine Lösung generierst.
- **n8n**: JSON oder Screenshot-Beschreibung liefern.
- **Power Automate**: Schritte beschreiben ("Apply to each", "Compose") oder Expressions.
- **Kestra**: YAML-Codeblock liefern.

### 3. Datenhandling
**Beschreibung:** Präzise Angaben zu Datentypen machen.
**Muss enthalten:**
- JSON-Struktur-Beispiele wo relevant.
- Explizite Erwähnung von Datentyp-Konvertierungen (z.B. String zu Integer).

## Ausgabeformat

### Für Workflow-Definitionen
```yaml
# Beispiel für Kestra / Allgemeine YAML-Darstellung
id: mein-flow
namespace: firma.team
tasks:
  - id: schritt-1
    type: ...
```

### Für Expressions
> Nutze `formatDateTime(utcNow(), 'yyyy-MM-dd')`, um das Datum zu formatieren.

## Häufige Fehler
❌ **Fehler:** Eine Screenshot-Beschreibung liefern, ohne die Konfiguration innerhalb der Schritte zu erklären.
✅ **Stattdessen:** "Füge eine 'Filter Array'-Aktion hinzu. Setze 'From' auf `outputs('Get_items')` und die Bedingung auf `item()?['Status']` gleich `Active`."
