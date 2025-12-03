---
description: Umfassender Leitfaden zur Fehlerbehandlung, Debugging und Erstellung robuster Workflows in n8n.
globs: **/*.json
---

# Handbuch für Fehlerbehandlung & Debugging

Robuste Workflows zu bauen bedeutet anzunehmen, dass Dinge schiefgehen *werden*. n8n bietet einen mehrschichtigen Ansatz zur Fehlerbehandlung, von einfachen Wiederholungen bis hin zu komplexen globalen Fehlerbehandlern.

## 1. Die drei Verteidigungsebenen

### Ebene 1: Node Retries (Vorübergehende Fehler)
Am besten für Netzwerkaussetzer (z. B. API-Timeout, 503 Service Unavailable).
*   **Mechanismus**: Der Node versucht es automatisch erneut, bevor er fehlschlägt.
*   **Konfiguration**: In Node Settings -> **Retry On Fail**.
    *   *Max Tries*: z. B. 3.
    *   *Wait Between Tries*: z. B. 5000ms.
*   **Unterstützte Nodes**: HTTP Request und viele eingebaute Integrations-Nodes.

### Ebene 2: Node "Continue On Fail" (Logikfehler)
Am besten für erwartete Fehler (z. B. "Benutzer nicht gefunden", "Datei fehlt"), bei denen der Workflow entscheiden soll, was als nächstes zu tun ist.
*   **Mechanismus**: Der Node schlägt fehl, aber der Workflow läuft weiter. Der Node gibt Fehler-Metadaten aus.
*   **Konfiguration**: In Node Settings -> **On Error** -> **Continue**.
*   **Ausgabe**: Der Node fügt ein `error`-Objekt zum Ausgabe-JSON hinzu.
    ```json
    {
      "json": {
        "error": {
          "message": "404 - Not Found",
          "description": "The resource could not be found."
        }
      }
    }
    ```

### Ebene 3: Globaler Fehler-Workflow (Katastrophale Fehler)
Am besten für unbehandelte Abstürze (z. B. OOM, Syntaxfehler, Datenbank ausgefallen).
*   **Mechanismus**: Wenn ein Workflow abstürzt, löst n8n einen separaten "Error Workflow" aus.
*   **Verwendung**: Senden von Alarmen (Slack/PagerDuty) an das Entwicklerteam.

---

## 2. Implementierung des "Try-Catch" Musters

n8n hat keinen nativen `Try-Catch`-Block, aber Sie können ihn mit **Continue On Fail** simulieren.

### Das Muster
1.  **Aktions-Node** (Das "Try"):
    *   Aktivieren Sie **Continue On Fail**.
2.  **IF Node** (Das "Catch"):
    *   **Bedingung**: `{{ $json.error }}` *Is Not Empty*.
3.  **True-Zweig** (Fehlerbehandler):
    *   Behandeln Sie den Fehler (z. B. Benutzer erstellen, wenn "Benutzer nicht gefunden"-Fehler).
4.  **False-Zweig** (Erfolg):
    *   Fahren Sie mit dem normalen Ablauf fort.

---

## 3. Der Globale Fehler-Workflow

Dies ist für die Produktion obligatorisch. Er fängt alles ab, was Sie in Ebene 1 und 2 übersehen haben.

### Einrichtung
1.  Erstellen Sie einen neuen Workflow namens **"Global Error Handler"**.
2.  **Trigger**: Fügen Sie einen **Error Trigger** Node hinzu.
3.  **Aktion**: Fügen Sie einen Messaging-Node hinzu (Slack, Teams, E-Mail).
4.  **Nachricht konfigurieren**:
    *   **Workflow**: `{{ $execution.workflow.name }}`
    *   **Execution ID**: `{{ $execution.id }}`
    *   **Error Message**: `{{ $execution.error.message }}`
    *   **Link**: `{{ $execution.url }}` (Direkter Link zur fehlgeschlagenen Ausführungs-UI).
5.  **Verknüpfung**: Gehen Sie zu Ihrem **Haupt-Workflow** -> Settings -> **Error Workflow** -> Wählen Sie "Global Error Handler".

### Welche Daten sind verfügbar?
Der **Error Trigger** Node gibt reichhaltigen Kontext aus:
*   `execution`: ID, URL, Modus (manual/production).
*   `workflow`: ID, Name.
*   `error`: Nachricht, Stack Trace, Name des fehlgeschlagenen Nodes.

---

## 4. Fehlerweiterleitung in Sub-Workflows

Bei Verwendung von `Execute Workflow` verhalten sich Fehler anders.

### Standardverhalten
Wenn der Sub-Workflow fehlschlägt, schlägt der Eltern-Workflow sofort am `Execute Workflow` Node fehl. Der Globale Fehlerbehandler des **Elternteils** wird ausgelöst.

### Fehler hochreichen (Graceful Failure)
Wenn Sie möchten, dass der Elternteil den Fehler des Kindes logisch behandelt (z. B. "Wenn Kind fehlschlägt, versuche Kind B"):

1.  **Im Sub-Workflow**:
    *   Fügen Sie einen **Error Trigger** Node hinzu.
    *   Verbinden Sie ihn mit einem **Set** Node.
    *   Ausgabe: `{ "success": false, "error": "Etwas ist schiefgelaufen" }`.
    *   **Wichtig**: Dies lässt den Sub-Workflow (technisch gesehen) "erfolgreich" beenden, gibt aber ein Fehlerobjekt zurück.
2.  **Im Eltern-Workflow**:
    *   Der `Execute Workflow` Node gibt das `{ "success": false }` Objekt aus.
    *   Verwenden Sie einen **IF** Node, um `success` zu prüfen.

---

## 5. Fortgeschrittene Muster

### Manuelle Wiederholungsschleife (Wait + Goto)
Für Nodes, die kein natives "Retry On Fail" unterstützen (z. B. Code Node Logik).

1.  **Node A**: Die Operation.
2.  **IF Node**: War es erfolgreich?
    *   **Ja**: Weiter.
    *   **Nein**:
        1.  **Wait Node**: Warte 5 Sekunden.
        2.  **Verbinden** Sie den Wait Node zurück zum Eingang von **Node A**.
*   **Warnung**: Dies erzeugt eine Endlosschleife, wenn es nie erfolgreich ist. Fügen Sie einen Zähler in einem Code Node hinzu, um Wiederholungen zu begrenzen (z. B. max 5).

### Der "Stop and Error" Node
Manchmal möchten Sie den Workflow absichtlich abstürzen lassen, um den Globalen Fehlerbehandler auszulösen.
*   **Anwendungsfall**: Validierung. "Wenn `amount` negativ ist, ist dies eine kritische Datenkorruption. Stoppe alles."
*   **Node**: **Stop and Error**.
*   **Konfig**: Setzen Sie die Fehlermeldung. Diese Nachricht wird an den Fehler-Workflow gesendet.

### Circuit Breaker (Static Data)
Verhindern Sie, dass ein kaputter Workflow Ihr Slack 10.000 Mal zuspammt.

```javascript
// Code Node am Anfang des Fehler-Workflows
const staticData = getWorkflowStaticData('global');
const lastAlert = staticData.lastAlertTime || 0;
const now = new Date().getTime();

// Nur einmal alle 30 Minuten alarmieren
if (now - lastAlert < 1800000) {
  return []; // Ausführung stoppen (kein Alarm)
}

staticData.lastAlertTime = now;
return items;
```

---

## 6. Debugging-Tipps

### Daten pinnen (Pinning Data)
*   **Szenario**: Sie bauen die Mitte eines Workflows und möchten nicht jedes Mal den Webhook neu auslösen.
*   **Aktion**: Führen Sie den Workflow einmal aus. Klicken Sie auf den Node vor dem, den Sie bauen. Klicken Sie auf **"Pin Data"**.
*   **Ergebnis**: Sie können nun die nachfolgenden Nodes sofort mit den gespeicherten Daten ausführen.

### Debugging von Webhooks
*   **Webhook Test URL**: `.../webhook-test/...`
    *   Wartet darauf, dass Sie in der UI auf "Execute" klicken.
    *   Zeigt die Daten sofort in der UI an.
*   **Webhook Prod URL**: `.../webhook/...`
    *   Läuft im Hintergrund.
    *   Daten nur in der "Executions"-Liste sichtbar.

### Der "No Op" Node
*   n8n hat einen **No Operation, do nothing** Node.
*   **Anwendungsfall**: Verwenden Sie ihn als Platzhalter, um Zweige zusammenzuführen oder Teile eines Flusses vorübergehend zu trennen, ohne Verbindungen zu löschen.
