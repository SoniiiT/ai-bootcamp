---
description: Umfassender Leitfaden zur n8n-Workflow-Struktur, Einstellungen, Lebenszyklus und Best Practices.
applyTo: "**"
---

# Workflow-Handbuch

Ein Workflow ist eine Sammlung von Nodes, die verbunden sind, um einen Prozess zu automatisieren. Dieser Leitfaden behandelt den Lebenszyklus, die Konfiguration und die Optimierung von Workflows.

## 1. Workflow-Struktur

### Der Trigger
Jeder Workflow beginnt mit einem **Trigger Node**.
*   **Webhook**: Wird ausgeführt, wenn eine URL aufgerufen wird.
*   **Schedule**: Wird zu einer bestimmten Zeit ausgeführt (Cron).
*   **App Trigger**: Wird ausgeführt, wenn ein Ereignis in einer App passiert (z. B. "Slack - On New Message").
*   **Manual**: Wird ausgeführt, wenn Sie auf "Execute Workflow" klicken (verwendet für Tests oder Sub-Workflows).

### Der Fluss
*   **Linear**: A -> B -> C.
*   **Verzweigung**: A -> IF -> (B oder C).
*   **Looping**: A -> Loop -> B -> Loop.

---

## 2. Workflow-Einstellungen

Diese korrekt zu konfigurieren ist entscheidend für die Produktionsstabilität. Zugriff über **Settings** (oben rechts).

### Ausführungsdaten (Datenbank-Bloat)
*   **Save Execution Progress**: **DEAKTIVIEREN SIE DIES**.
    *   *Warum*: Wenn aktiviert, schreibt n8n die volle JSON/Binary-Payload nach *jedem einzelnen Node* in die DB. Dies tötet die Leistung und füllt die Festplatte.
    *   *Ausnahme*: Nur aktivieren, während ein spezifischer Absturz debuggt wird.
*   **Save Data Error Execution**: **Aktivieren**. Sie benötigen dies, um Fehler zu debuggen.
*   **Save Data Success Execution**: **Deaktivieren** für Workflows mit hohem Volumen.

### Fehlerbehandlung
*   **Error Workflow**: Wählen Sie einen Workflow aus, der ausgeführt werden soll, wenn dieser fehlschlägt.
    *   *Verwendung*: Senden Sie einen Slack-Alarm mit der Ausführungs-ID und der Fehlermeldung.
*   **Timeouts**: Setzen Sie ein **Timeout After** (z. B. 1 Stunde).
    *   *Warum*: Verhindert, dass "Zombie"-Ausführungen ewig laufen, wenn eine API hängt.

### Zeitzone
*   **Standard**: Verwendet die Instanz-Zeitzone (`GENERIC_TIMEZONE`).
*   **Override**: Setzen Sie eine spezifische Zeitzone, wenn Ihr Zeitplan von einer bestimmten Region abhängt (z. B. "9 Uhr Tokio-Zeit").

---

## 3. Aktivierung & Lebenszyklus

### Aktiv vs Inaktiv
*   **Inaktiv**: Trigger sind deaktiviert. Webhooks geben 404 zurück (außer bei Verwendung der Test-URL).
*   **Aktiv**: Trigger sind live.

### Webhook-URLs
*   **Test URL**: `https://.../webhook-test/...`
    *   Funktioniert nur, wenn Sie die UI geöffnet haben und auf "Execute Workflow" klicken.
    *   Gut zum Debuggen.
*   **Production URL**: `https://.../webhook/...`
    *   Funktioniert nur, wenn der Workflow **Aktiv** ist.
    *   Zeigt Daten nicht sofort in der UI an (prüfen Sie die Execution History).

---

## 4. Fehlerbehandlungsstrategien

### Node-Ebene
*   **Continue On Fail**: Aktivieren Sie dies in den Einstellungen eines Nodes, um zu verhindern, dass der Workflow stoppt.
*   **Error Output**: Wenn "Continue On Fail" aktiviert ist, fügt der Node eine `error`-Eigenschaft zur Ausgabe hinzu. Verwenden Sie einen **IF** Node, um darauf zu prüfen.

### Workflow-Ebene
*   **Error Trigger Node**: Erstellen Sie einen separaten "Error Handler"-Workflow.
    *   Trigger: **Error Trigger**.
    *   Aktion: Senden Sie E-Mail/Slack.
    *   Konfig: Setzen Sie diesen Workflow als den "Error Workflow" in Ihren Haupt-Workflows.

---

## 5. Debugging & Entwicklung

### Pinning Data
Sie können Daten an einen Node "pinnen", um Eingaben zu simulieren, ohne den Trigger erneut auszuführen.
1.  Führen Sie den Workflow einmal aus, um Daten zu erhalten.
2.  Klicken Sie auf den Node -> "Pin Data".
3.  Jetzt können Sie nachfolgende Nodes bearbeiten und sie sofort mit den gepinnten Daten ausführen.

### Execution History
*   **Filter**: Zeigen Sie nur "Error"-Ausführungen an.
*   **Retry**: Sie können eine fehlgeschlagene Ausführung über die UI wiederholen.
    *   *Hinweis*: Es wird mit den *ursprünglichen* Daten wiederholt. Wenn Sie einen Fehler im Workflow behoben haben, schlägt die Wiederholung möglicherweise immer noch fehl, wenn die Logik nicht auf den alten Ausführungskontext angewendet wurde (abhängig davon, wo er fehlgeschlagen ist).

---

## 6. Nebenläufigkeit & Limits

### Globales Limit
*   `N8N_CONCURRENCY_PRODUCTION_LIMIT`: Maximale aktive Ausführungen.
*   Wenn überschritten, werden neue Ausführungen in die Warteschlange gestellt (oder abgelehnt, wenn die Warteschlange voll ist).

### Workflow-Limit
*   Sie können die Nebenläufigkeit pro Workflow in der UI nicht strikt begrenzen.
*   **Workaround**: Verwenden Sie einen **Redis**- oder **Postgres**-Lock in einem Code Node am Anfang des Workflows, wenn Sie eine Singleton-Ausführung sicherstellen müssen.

---

## 7. Best Practices

1.  **Idempotenz**: Entwerfen Sie Workflows so, dass sie sicher wiederholt werden können.
    *   *Schlecht*: "Benutzer erstellen". (Wiederholung = Doppelter Benutzer).
    *   *Gut*: "Prüfen, ob Benutzer existiert -> Erstellen, wenn nicht".
2.  **Anmerkungen**: Verwenden Sie **Notes** (Haftnotizen) auf der Leinwand, um komplexe Logik zu erklären.
3.  **Sauberes Layout**: Verwenden Sie "Auto-align", um den Graphen lesbar zu halten.
4.  **Benennung**: Benennen Sie Ihre Routen in Switch Nodes (z. B. "Ist Kunde" vs "Ist Lead").
