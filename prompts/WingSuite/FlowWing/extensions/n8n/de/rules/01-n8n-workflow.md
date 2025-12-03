```markdown
# Regel 1 - n8n Workflow-Generierung

## Zweck
Stellt sicher, dass n8n-Workflows in einem Format generiert werden, das direkt verwendbar (Copy & Paste), sicher und technisch korrekt gemäß der spezifischen Architektur von n8n ist.

## Kernanforderungen

### 1. Ausgabeformat: JSON Copy & Paste
**Beschreibung:** Die primäre Ausgabe für die Workflow-Logik muss das n8n-JSON-Format sein.
**Implementierung:**
- Stellen Sie den Workflow-Code in einem JSON-Codeblock bereit.
- Stellen Sie sicher, dass die JSON-Struktur gültig ist (enthält `nodes` und `connections`).
- **Benutzeranweisung**: Sagen Sie dem Benutzer ausdrücklich: "Sie können diesen JSON-Code kopieren und direkt in Ihren n8n-Editor einfügen (Strg+V)."

**Beispiel:**
```json
{
  "nodes": [
    {
      "parameters": {},
      "name": "Start",
      "type": "n8n-nodes-base.start",
      "typeVersion": 1,
      "position": [250, 300]
    }
  ],
  "connections": {}
}
```

### 2. Technische Genauigkeit & Datenstruktur
**Beschreibung:** Sie müssen sich strikt an die interne Datenstruktur von n8n und die in den Kontextdateien definierten Best Practices halten.
**Implementierung:**
- **Code Node**: Konsultieren Sie `code-node.md`.
    - **Input**: Verwenden Sie `$input.all()` (v1-Stil) oder `items` (Legacy).
    - **Output**: Geben Sie IMMER ein Array von Objekten zurück, die in den `json`-Schlüssel verpackt sind: `return [{ json: { myField: "value" } }];`.
    - **Daten**: Verwenden Sie Luxon (`DateTime`, `$now`) für alle Datumsberechnungen. Verwenden Sie NICHT das native `Date`-Objekt, es sei denn, es ist notwendig.
- **Expressions**: Konsultieren Sie `expressions.md`.
    - **Syntax**: Verwenden Sie `{{ $json.field }}`.
    - **Looping**: Verwenden Sie die "Paired Item"-Syntax `{{ $('Node').item.json.id }}`, wenn Sie Nodes innerhalb einer Schleife/Merge referenzieren, um eine korrekte Item-Zuordnung sicherzustellen.
- **AI Agents**: Konsultieren Sie `ai-advanced.md`.
    - **Architektur**: Verwenden Sie den **AI Agent** Node als Root.
    - **Verbindungen**: Sie MÜSSEN die Sub-Nodes (Chat Model, Memory, Tools) explizit mit den spezifischen **Input**-Konnektoren des Agent-Nodes im JSON verbinden.
- **Binary Data**: Konsultieren Sie `binary-data.md`.
    - **Handhabung**: Verwenden Sie die `binary`-Eigenschaft. Verwenden Sie den `Move Binary Data` Node, um von/zu JSON zu konvertieren.
- **Fehlerbehandlung**: Konsultieren Sie `error-handling.md`.
    - **Resilienz**: Konfigurieren Sie für externe API-Aufrufe "Continue On Fail" und prüfen Sie auf `error`-Ausgabe, oder schlagen Sie einen globalen Fehler-Workflow vor.

### 3. Produktionsreife
**Beschreibung:** Workflows sollten für Stabilität und Skalierung ausgelegt sein, nicht nur, um "einmal zu funktionieren".
**Implementierung:**
- **Umgebungsvariablen**: Konsultieren Sie `production.md`. Verwenden Sie `{{ $env.MY_VAR }}` für Secrets/Konfiguration anstelle von fest codierten Strings.
- **Zustandsmanagement**: Konsultieren Sie `platform.md`. Verwenden Sie `staticData` für Polling-Trigger, um die Verarbeitung von Duplikaten zu vermeiden.
- **Ratenbegrenzung**: Konsultieren Sie `integrations.md`. Verwenden Sie "Split In Batches" + "Wait" Node für ratenbegrenzte APIs. Erstellen Sie KEINE engen Schleifen ohne Verzögerungen.
- **Optimierung**: Konsultieren Sie `workflows.md`. Raten Sie Benutzern, "Save Execution Progress" für Workflows mit hohem Volumen zu deaktivieren.

### 4. Umgang mit Credentials & APIs
**Beschreibung:** Credentials dürfen NIEMALS Teil der JSON-Ausgabe sein.
**Implementierung:**
- **Credentials**: Konsultieren Sie `credentials.md`. Erklären Sie genau, welcher Typ verwendet werden soll (Predefined vs Generic).
- **Integrationen**: Konsultieren Sie `integrations.md`. Bevorzugen Sie Built-in Nodes. Verwenden Sie "Custom API Call" innerhalb von Built-in Nodes für fehlende Operationen, bevor Sie zum HTTP Request wechseln.
- **HTTP Requests**: Konsultieren Sie `http-request.md`. Verwenden Sie korrekte Authentifizierungs- und Paginierungseinstellungen.
- **Im Text**: Stellen Sie eine **Schritt-für-Schritt-Anleitung** getrennt vom Codeblock bereit, die erklärt, wie die Credentials eingerichtet werden.
- **Anweisung**: "Bitte erstellen Sie ein neues Credential vom Typ '[Typ]' und geben Sie Ihren API-Schlüssel ein."

### 5. Node-Konfiguration
- **Code Node**: Wenn komplexe Logik erforderlich ist, bevorzugen Sie den `Code` Node (JavaScript) gegenüber mehreren komplexen IF/Switch Nodes, wenn dies die Ansicht vereinfacht.
- **Benennung**: Geben Sie Nodes aussagekräftige Namen (z. B. "Get Customer Data" anstelle von "HTTP Request").
- **Layout**: Wenn Sie Positionen generieren, versuchen Sie, diese logisch anzuordnen (von links nach rechts).

## Antwortstruktur für n8n-Anfragen

1. **Workflow-Übersicht**: Kurze Erklärung, was der Flow tut.
2. **Voraussetzungen**: Liste der benötigten Credentials (z. B. "Sie benötigen ein Slack API Token").
3. **Der Workflow (JSON)**: Der kopierbare Codeblock.
4. **Einrichtungsanweisungen**:
   - Wie die Credentials konfiguriert werden.
   - Alle spezifischen Umgebungsvariablen oder Einstellungen.
   - **Testen**: Schlagen Sie vor, "Pin Data" oder einen "Manual Trigger" zu verwenden, um den Flow sicher zu testen.
```
