---
description: Umfassender Leitfaden zur Modularisierung von Workflows unter Verwendung von Sub-Workflows in n8n.
globs: **/*.json
---

# Modularisierungs- & Sub-Workflow-Handbuch

Wenn n8n-Projekte wachsen, werden monolithische Workflows schwer zu warten. Modularisierung beinhaltet das Aufteilen von Logik in wiederverwendbare **Sub-Workflows**.

## Kernkomponenten

### 1. Der Aufrufer: `Execute Workflow` Node
Dieser Node befindet sich im **Parent Workflow** (Eltern-Workflow) und ruft den Sub-Workflow auf.

#### Ausführungsmodi
*   **Run Once for All Items (Batch Mode)**:
    *   **Verhalten**: Sendet alle Eingabeelemente als ein einziges Array an den Sub-Workflow. Der Sub-Workflow läuft *einmal*.
    *   **Anwendungsfall**: Aggregation (z. B. "Erstelle einen täglichen Bericht aus diesen 100 Zeilen") oder wenn der Sub-Workflow sein eigenes Looping handhabt.
    *   **Leistung**: Hoch. Bevorzugt für große Datenmengen.
*   **Run for Each Item (Loop Mode)**:
    *   **Verhalten**: Der Sub-Workflow läuft *separat* für jedes einzelne Eingabeelement.
    *   **Anwendungsfall**: Isolation. Wenn Element 5 fehlschlägt, sind Elemente 1-4 und 6-10 trotzdem erfolgreich. Komplexe Logik pro Element.
    *   **Leistung**: Niedrig. Kann Overhead verursachen, wenn Tausende von Elementen verarbeitet werden.

### 2. Der Aufgerufene: `Execute Workflow Trigger` Node
Dieser Node ersetzt den Standard-Trigger (Webhook/Schedule) im **Sub-Workflow**.
*   **Eingabe**: Empfängt die JSON- und Binärdaten, die vom Eltern-Workflow übergeben wurden.
*   **Ausgabe**: Startet den Ausführungsfluss.

---

## Datenübergabe

### Daten nach unten geben (Parent -> Child)
*   **JSON**: Was auch immer als JSON in den `Execute Workflow` Node eintritt, ist im `Execute Workflow Trigger` Node verfügbar.
*   **Variablen**: Sub-Workflows erben **NICHT** automatisch Variablen oder Umgebungsdaten vom Eltern-Workflow. Sie müssen diese explizit als JSON-Felder übergeben.
    *   *Muster*: Verwenden Sie einen `Set` Node vor dem `Execute Workflow` Node, um Konfigurationsfelder hinzuzufügen (z. B. `{ "operation": "create_user", "dryRun": true }`).

### Daten nach oben geben (Child -> Parent)
*   **Implizite Rückgabe**: Die Ausgabe des **zuletzt ausgeführten Nodes** im Sub-Workflow wird an den Eltern-Workflow zurückgegeben.
*   **Explizite Rückgabe**: Es ist Best Practice, Ihren Sub-Workflow mit einem `Set` Node namens "Output" zu beenden, um klar zu definieren, was zurückgegeben wird.
    *   *Beispiel*: `{ "success": true, "id": 123 }`

---

## Fehlerbehandlung in Sub-Workflows

Wenn ein Sub-Workflow fehlschlägt, hängt das Verhalten von den Einstellungen des `Execute Workflow` Nodes ab.

### Standardverhalten
Wenn der Sub-Workflow einen Fehler verursacht, schlägt der `Execute Workflow` Node im Eltern-Workflow fehl und stoppt den Eltern-Workflow.

### "Continue On Fail" Muster
Um den Eltern-Workflow robust zu machen:
1.  **Parent**: Aktivieren Sie "Continue On Fail" in den Einstellungen des `Execute Workflow` Nodes.
2.  **Child**: Verwenden Sie einen **Error Trigger** Node innerhalb des Sub-Workflows.
    *   Verbinden Sie den Error Trigger mit einem `Set` Node, der `{ "error": true, "message": "..." }` ausgibt.
    *   Dies ermöglicht es dem Sub-Workflow, "elegant zu scheitern" und ein Fehlerobjekt zurückzugeben, anstatt den Eltern-Workflow zum Absturz zu bringen.

---

## Gängige Muster

### 1. Der "Router" Workflow
Ein Haupt-Workflow agiert als Verkehrsleiter.
1.  **Trigger**: Webhook empfängt eine Anfrage.
2.  **Switch Node**: Prüft `body.type`.
3.  **Routen**:
    *   Typ "order": Rufe `Sub-Workflow: Process Order`.
    *   Typ "refund": Rufe `Sub-Workflow: Process Refund`.
    *   Typ "support": Rufe `Sub-Workflow: Create Ticket`.

### 2. Der "Utility" Workflow
Eine wiederverwendbare Funktion, die von vielen Workflows genutzt wird.
*   *Beispiele*: "Firmendaten anreichern", "Slack-Benachrichtigung senden (Standardisiert)", "Loggen in Splunk".
*   *Vorteil*: Ändern Sie die Logging-Logik an einer Stelle, und alle 50 Workflows werden sofort aktualisiert.

### 3. Rekursive Workflows (Paginierung)
n8n erlaubt es einem Workflow, sich selbst aufzurufen. Dies ist nützlich für die Paginierung von APIs, die keine Standardmethoden unterstützen.
1.  **Trigger**: Empfängt `page` Nummer (Standard 1).
2.  **HTTP Request**: Ruft Daten für `page` ab.
3.  **IF Node**: Gibt es mehr Daten?
    *   **Ja**: Rufe `Execute Workflow` (Selbst) mit `page + 1`.
    *   **Nein**: Fertig.

---

## Testen & Debugging

### Sub-Workflows unabhängig testen
Sie können einen Sub-Workflow nicht einfach über die UI "Ausführen", wenn er von Daten eines Eltern-Workflows abhängt.
*   **Lösung**: Fügen Sie einen **Manual Trigger** Node neben dem `Execute Workflow Trigger` hinzu.
*   **Mock-Daten**: Konfigurieren Sie im Manual Trigger "Test Input" JSON, das nachahmt, was der Eltern-Workflow senden würde. Dies ermöglicht es Ihnen, den Sub-Workflow isoliert zu entwickeln und zu testen.

### Debugging
*   **Ausführungsliste**: Wenn Sie die Ausführung eines Eltern-Workflows ansehen, bietet n8n einen Link zur spezifischen Ausführungs-ID des Sub-Workflows. Klicken Sie darauf, um genau zu sehen, was im Kind-Workflow passiert ist.

---

## Best Practices

1.  **Benennung**: Präfixen Sie Sub-Workflows klar (z. B. `[Sub] Process User`, `[Util] Send Email`).
2.  **Dokumentation**: Verwenden Sie die "Notes"-Funktion im `Execute Workflow Trigger`, um das erwartete Eingabeschema zu dokumentieren (z. B. "Erfordert `userId` und `email`").
3.  **Atomare Logik**: Halten Sie Sub-Workflows auf eine einzelne Aufgabe fokussiert.
4.  **Tiefe Verschachtelung vermeiden**: Einen Sub-Workflow aufzurufen, der einen weiteren Sub-Workflow aufruft, ist in Ordnung, aber 5 Ebenen tief zu gehen, macht das Debugging zum Albtraum. Halten Sie es wenn möglich flach.
