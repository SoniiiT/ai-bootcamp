```markdown
# Troubleshooting & Debugging Handbuch

Dieser Leitfaden bietet umfassende Strategien zur Identifizierung, Diagnose und Behebung von Problemen in n8n-Workflows, von einfachen Logikfehlern bis hin zu komplexen serverseitigen Ausfällen.

## 1. Debugging-Tools & Techniken

### Pin Data (Der "Zeitsparer")
Pinning ermöglicht es Ihnen, die Ausgabedaten eines Nodes "einzufrieren".
*   **Warum**: Verhindert das erneute Ausführen schwerer Trigger oder teurer API-Aufrufe während der Entwicklung.
*   **Wie**:
    1.  Führen Sie den Workflow einmal aus.
    2.  Klicken Sie auf das "Pin Data"-Symbol (Reißzwecke) im Ausgabepanel des Nodes.
    3.  Wenn Sie nun auf "Execute Workflow" klicken, startet n8n *nach* dem gepinnten Node und verwendet die zwischengespeicherten Daten.
*   **Warnung**: Denken Sie daran, das Pinning vor dem Deployment in die Produktion aufzuheben, wenn die Daten dynamisch sein müssen!

### Execution Inspector
Das primäre Tool für die Post-Mortem-Analyse.
*   **Zugriff**: Klicken Sie in der linken Seitenleiste auf "Executions".
*   **Filter**: Filtern Sie nach "Error", "Success" oder spezifischer Workflow-ID.
*   **Deep Dive**:
    *   Klicken Sie auf eine Ausführung, um sie zu öffnen.
    *   Klicken Sie auf *jeden beliebigen Node*, um den **Input** (was er empfangen hat) und den **Output** (was er produziert hat) zu sehen.
    *   **Binärdaten**: Sie können Dateien, die in vergangenen Ausführungen verarbeitet wurden, herunterladen, um ihren Inhalt zu überprüfen.

### Console Logging (Code Nodes)
*   **Browser-Konsole**: Bei manueller Ausführung in der UI erscheint die Ausgabe von `console.log()` in den Entwicklertools Ihres Browsers (F12 > Konsole).
    *   *Tipp*: Verwenden Sie `console.log('MyVar:', JSON.stringify(myVar))`, um vollständige Objekte zu sehen.
*   **Server-Logs**: In der Produktion (Trigger-Läufe) geht die Ausgabe von `console.log()` in die n8n-Server-Logs (Docker-Logs).

### "Debug in Editor"
Eine leistungsstarke Funktion, um fehlgeschlagene Produktionsläufe zu beheben.
1.  Öffnen Sie eine fehlgeschlagene Ausführung.
2.  Klicken Sie auf **"Debug in Editor"**.
3.  n8n lädt die *exakten Daten* dieses Fehlers in Ihre aktuelle Editor-Sitzung.
4.  Sie können nun die Node-Einstellungen anpassen und nur diesen Schritt erneut ausführen, um die Logik zu korrigieren.

## 2. Häufige Fehlermeldungen & Lösungen

### "Referenced node is unexecuted"
*   **Kontext**: Sie haben einen Ausdruck wie `$('Node A').item.json.id` verwendet.
*   **Ursache**: "Node A" befindet sich in einem Zweig, der nicht ausgeführt wurde (z. B. der `False`-Pfad eines IF Nodes), oder er wurde *noch* nicht ausgeführt.
*   **Lösung**:
    *   Stellen Sie sicher, dass die Logik garantiert, dass "Node A" vor dem aktuellen Node ausgeführt wird.
    *   Verwenden Sie einen **Merge Node**, um Zweige wieder zusammenzuführen.
    *   Verwenden Sie defensive Programmierung: `{{ $('Node A').item ? $('Node A').item.json.id : null }}`.

### "JSON parameter need to be an object"
*   **Kontext**: Code Node Ausgabe.
*   **Ursache**: Sie haben ein Array von Strings/Zahlen/Nulls zurückgegeben. n8n *erfordert* Objekte.
*   **Lösung**: Verpacken Sie Werte in `json`: `return myStrings.map(s => ({ json: { value: s } }))`.

### "ECONNRESET" / "ETIMEDOUT" / "502 Bad Gateway"
*   **Kontext**: HTTP Request Node.
*   **Ursache**: Die externe API ist down, langsam oder blockiert Sie.
*   **Lösung**:
    *   **Retry**: Aktivieren Sie "Retry on Fail" in den Node-Einstellungen (z. B. 3 Wiederholungen).
    *   **Timeout**: Erhöhen Sie die "Timeout"-Einstellung in den Node-Optionen.
    *   **Rate Limit**: Möglicherweise senden Sie zu schnell. Verwenden Sie "Split In Batches" + "Wait".

### "Memory Limit Exceeded" / "JavaScript heap out of memory"
*   **Kontext**: Verarbeitung großer JSON-Dateien oder SQL-Dumps.
*   **Ursache**: n8n lädt alle Daten in den RAM. Eine 100MB JSON-Datei kann 500MB RAM zum Parsen benötigen.
*   **Lösung**:
    *   **Split In Batches**: Verarbeiten Sie 50 Elemente auf einmal, nicht 50.000.
    *   **Felder optimieren**: Wählen Sie in SQL/HTTP Nodes *nur* die Spalten aus, die Sie benötigen.
    *   **Sub-Workflows**: Lagern Sie die Verarbeitung in einen Worker-Workflow aus.

### "No binary data property"
*   **Kontext**: Hochladen von Dateien oder Bearbeiten von Bildern.
*   **Ursache**: Der Node erwartet eine Binäreigenschaft namens `data` (Standard), aber Ihre vorherige Node-Ausgabe nannte sie `file` oder `attachment`.
*   **Lösung**: Überprüfen Sie die Einstellung "Input Data Field Name" und passen Sie sie an den tatsächlichen Namen der eingehenden Eigenschaft an.

## 3. Fehlerbehandlungsstrategien

### Level 1: "Continue On Error" (Resilienz)
*   **Einstellung**: In den Node-Einstellungen (Zahnrad-Symbol) > "On Error" > "Continue".
*   **Verhalten**: Wenn der Node fehlschlägt, gibt er ein JSON-Objekt mit `error: "message"` aus. Der Workflow läuft weiter.
*   **Anwendungsfall**: Verarbeitung einer Liste von 100 E-Mails. Wenn eine fehlschlägt, sollen die anderen 99 nicht stoppen.
*   **Muster**:
    1.  HTTP Request (Continue On Error).
    2.  IF Node: `{{ $json.error }}` existiert?
    3.  True: Fehler in DB loggen.
    4.  False: Verarbeitung fortsetzen.

### Level 2: Error Trigger (Globaler Handler)
*   **Konzept**: Ein "Auffang"-Workflow, der ausgeführt wird, wann immer *irgendein* Workflow fehlschlägt.
*   **Einrichtung**:
    1.  Erstellen Sie einen neuen Workflow.
    2.  Fügen Sie einen **Error Trigger** Node hinzu.
    3.  Fügen Sie einen **Slack/Email** Node hinzu, um den Alarm zu senden.
    4.  Wählen Sie in Ihrem Haupt-Workflow > Settings > **Error Workflow** diesen neuen Workflow aus.
*   **Verfügbare Daten**: Der Error Trigger liefert:
    *   `workflow.id`, `workflow.name`
    *   `execution.id`, `execution.url`
    *   `node.name` (welcher Node fehlgeschlagen ist)
    *   `error.message`, `error.stack`

### Level 3: Try/Catch Muster (Lokaler Handler)
*   **Konzept**: Fehler für einen bestimmten Logikabschnitt behandeln.
*   **Implementierung**:
    *   n8n hat keinen nativen "Try/Catch"-Block.
    *   Verwenden Sie **Continue On Error** + **IF Node** (wie in Level 1 beschrieben), um dies zu simulieren.

## 4. Verbindungsprobleme (Self-Hosted)

### "Self signed certificate in certificate chain"
*   **Ursache**: Ihre n8n-Instanz versucht, mit einem internen Server mit einem selbstsignierten SSL-Zertifikat zu kommunizieren.
*   **Lösung**:
    *   **Option A**: Setzen Sie im HTTP Request Node "Ignore SSL Issues" auf True.
    *   **Option B**: Setzen Sie die Umgebungsvariable `NODE_TLS_REJECT_UNAUTHORIZED=0` (Global, weniger sicher).

### Proxy-Konfiguration
Wenn n8n hinter einer Unternehmens-Firewall steht:
*   **Env Vars**:
    *   `HTTP_PROXY`, `HTTPS_PROXY`: Standard-Proxy-Einstellungen.
    *   `N8N_PROXY_RULES`: Granulare Kontrolle (z. B. Proxy für interne IPs umgehen).
```
