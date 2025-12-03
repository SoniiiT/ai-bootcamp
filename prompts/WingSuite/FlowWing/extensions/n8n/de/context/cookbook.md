---
description: Eine Sammlung fortgeschrittener Muster, Rezepte und Code-Snippets für n8n.
globs: **/*.json
---

# n8n Kochbuch & Rezepte

Dieser Leitfaden bietet Copy-Paste-Lösungen für häufige und fortgeschrittene n8n-Herausforderungen.

## 1. Fortgeschrittene Paginierung

### Szenario: Offset-basierte Paginierung
Die API erfordert `?limit=50&offset=0`, dann `offset=50`, usw.

**Muster**:
1.  **Set Node**: Initialisieren Sie `offset = 0` und `limit = 50`.
2.  **Loop Node (Code)**: Eine benutzerdefinierte Schleifenlogik.
    *   *Besserer Weg*: Verwenden Sie die eingebaute Paginierung des **HTTP Request** Nodes (verfügbar in neueren Versionen).
    *   *Manueller Weg*:
        1.  **HTTP Request**: Verwenden Sie `{{ $node["Set"].json.offset }}`.
        2.  **IF Node**: `{{ $json.length < 50 }}`?
            *   **True**: Ende.
            *   **False**: **Set Node** (`offset = offset + 50`) -> Verbinden Sie zurück zum HTTP Request.

### Szenario: Link Header Paginierung
Die API gibt die nächste URL im `Link`-Header zurück (z. B. GitHub).

**Muster**:
1.  **HTTP Request**: Aktivieren Sie "Pagination".
2.  **Pagination Mode**: "Response Header".
3.  **Header Name**: `link` oder `Link`.
4.  **Next URL Rule**: n8n parst standardmäßige RFC 5988 Link-Header normalerweise automatisch.

---

## 2. Fehlerbehandlungsmuster

### Der "Try-Catch" Block
n8n hat keinen nativen Try-Catch-Block, aber Sie können ihn simulieren.

**Muster**:
1.  **Node A (Das "Try")**: Aktivieren Sie "Continue On Fail".
2.  **IF Node**: Prüfen Sie `{{ $json.error !== undefined }}`.
    *   **True (Catch)**: Leiten Sie zur Fehlerbehandlungslogik weiter (Slack-Alarm, DB-Log).
    *   **False (Success)**: Fahren Sie mit dem normalen Ablauf fort.

### Der "Globale Fehlerbehandler"
**Muster**:
1.  Erstellen Sie einen separaten Workflow namens "Error Handler".
2.  **Trigger**: Error Trigger Node.
3.  **Aktion**: Senden Sie eine Slack-Nachricht mit:
    *   Workflow Name: `{{ $workflow.name }}`
    *   Execution ID: `{{ $execution.id }}`
    *   Error Message: `{{ $execution.error.message }}`
    *   Link: `{{ $execution.url }}`
4.  **Einrichtung**: Gehen Sie in jedem Produktions-Workflow zu Settings -> Error Workflow -> Wählen Sie "Error Handler".

---

## 3. Datendeduplizierung

### Szenario: Polling einer API, die kein Filtern nach Datum unterstützt
Sie rufen 100 Items ab, möchten aber nur die neuen seit dem letzten Lauf.

**Muster**:
1.  **Code Node**:
    ```javascript
    const staticData = getWorkflowStaticData('node');
    const lastId = staticData.lastId || 0;
    const newItems = items.filter(item => item.json.id > lastId);
    
    if (newItems.length > 0) {
      // Aktualisiere lastId auf die gefundene max ID
      staticData.lastId = Math.max(...newItems.map(i => i.json.id));
    }
    
    return newItems;
    ```

---

## 4. Dynamische Webhook-Antworten

### Szenario: Sofort 200 OK zurückgeben, dann verarbeiten
Sie möchten einen Webhook sofort bestätigen, um Timeouts zu vermeiden, und dann schwere Verarbeitung durchführen.

**Muster**:
1.  **Webhook Node**:
    *   **Response Mode**: "On Received".
    *   *Ergebnis*: Gibt sofort 200 OK zurück.
2.  **Verarbeitungs-Nodes**: Führen Sie Ihre schwere Arbeit durch (PDF-Generierung, KI, usw.).
    *   *Hinweis*: Der Aufrufer erhält NICHT das Ergebnis der Verarbeitung.

### Szenario: Bedingte Statuscodes zurückgeben
Geben Sie 400 zurück, wenn die Validierung fehlschlägt, 200 bei Erfolg.

**Muster**:
1.  **Webhook Node**:
    *   **Response Mode**: "Last Node".
2.  **IF Node**: Eingabe validieren.
    *   **True**: **Respond to Webhook** Node (Code: 200, Body: `{ "status": "ok" }`).
    *   **False**: **Respond to Webhook** Node (Code: 400, Body: `{ "error": "Ungültige Eingabe" }`).

---

## 5. Dateiverarbeitung

### Szenario: Entpacken, Verarbeiten, Wieder verpacken
**Muster**:
1.  **Read Binary File**: Laden Sie `archive.zip`.
2.  **Compression Node**: Aktion "Decompress".
    *   *Ausgabe*: Mehrere Items (eines pro Datei im Zip).
3.  **Filter**: Behalten Sie nur `.csv` Dateien.
4.  **Spreadsheet File**: CSV lesen.
5.  **... Daten verarbeiten ...**
6.  **Spreadsheet File**: CSV schreiben.
7.  **Compression Node**: Aktion "Compress".
    *   *Eingabe*: Alle Items.
    *   *Ausgabe*: Einzelnes `new_archive.zip`.

---

## 6. Fortgeschrittene Code-Snippets

### Verschachtelte Arrays flachklopfen
Verwandeln Sie `[{ "users": [A, B] }, { "users": [C, D] }]` in `[A, B, C, D]`.

```javascript
// Code Node
const allUsers = [];
for (const item of items) {
  allUsers.push(...item.json.users);
}
return allUsers.map(user => ({ json: user }));
```

### HMAC-Signatur generieren
Sichern Sie Ihre Webhooks durch Überprüfung von Signaturen.

```javascript
const crypto = require('crypto');
const secret = 'my-secret-key';
const body = JSON.stringify(items[0].json);
const signature = crypto.createHmac('sha256', secret).update(body).digest('hex');
return [{ json: { signature } }];
```

### Schlafen / Verzögerung (Zufällig)
Vermeiden Sie Bot-Erkennung, indem Sie eine zufällige Zeit schlafen.

```javascript
// Code Node
const waitTime = Math.floor(Math.random() * 5000) + 1000; // 1-6 Sekunden
await new Promise(resolve => setTimeout(resolve, waitTime));
return items;
```

---

## 7. Ratenbegrenzung (Token Bucket)

### Szenario: Strikt 1 Anfrage pro Sekunde
**Muster**:
1.  **Loop Over Items**: Batch Size = 1.
2.  **HTTP Request**: Führen Sie den Aufruf durch.
3.  **Wait Node**:
    *   **Resume**: "After Time Interval".
    *   **Amount**: 1.
    *   **Unit**: Seconds.
