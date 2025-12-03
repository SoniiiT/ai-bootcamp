# HTTP Request Node Handbuch

## Übersicht
Der HTTP Request Node ist das Kraftpaket von n8n und ermöglicht die Interaktion mit jeder REST-, GraphQL- oder SOAP-API. Er erfüllt zwei Hauptfunktionen:
1.  **Workflow-Schritt**: Abrufen oder Senden von Daten als Teil eines linearen Prozesses.
2.  **KI-Tool**: Agiert als Funktion, die ein KI-Agent dynamisch aufrufen kann.

## 1. Konfiguration & Authentifizierung

### Authentifizierungsmethoden
Sicherheit wird getrennt von der Node-Logik über Credentials gehandhabt.

| Methode | Anwendungsfall | Konfiguration |
| :--- | :--- | :--- |
| **Predefined** | **Bevorzugt**. Verwenden Sie dies, wenn n8n einen eingebauten Credential-Typ hat (z. B. Slack, Google). | Wählen Sie "Predefined Credential Type" -> Wählen Sie Dienst -> Wählen Sie Credential. Handhabt Token-Refresh automatisch. |
| **Header Auth** | APIs, die einen statischen API-Schlüssel in Headern erfordern. | Name: `Authorization` (oder `X-API-Key`), Wert: `Bearer <Key>`. |
| **Query Auth** | APIs, die den Schlüssel in der URL erfordern. | Name: `api_key`, Wert: `<Key>`. |
| **Basic Auth** | Legacy APIs. | Benutzername/Passwort in Base64 kodiert. |
| **OAuth2** | Komplexe Flows. | Erfordert Client ID, Secret, Auth URL, Token URL und Scopes. n8n handhabt die Callback-URL (`.../rest/oauth2-credential/callback`). |
| **Digest Auth** | Höhere Sicherheit als Basic. | Wird automatisch vom n8n Digest Auth Credential gehandhabt. |

### Anfrage-Konfiguration
*   **Methode**: GET, POST, PUT, DELETE, PATCH, HEAD.
*   **URL**: Unterstützt Ausdrücke (z. B. `https://api.example.com/users/{{ $json.userId }}`).
*   **Send Query Parameters**:
    *   **Specify Below**: Manuelles Hinzufügen von Schlüssel/Wert-Paaren.
    *   **JSON**: Übergeben Sie ein JSON-Objekt direkt (nützlich, wenn Parameter dynamisch sind).
    *   **Array Format**: Kritisch für PHP/Rails APIs.
        *   `No brackets`: `ids=1&ids=2`
        *   `Brackets []`: `ids[]=1&ids[]=2`
        *   `Indices [0]`: `ids[0]=1&ids[1]=2`

### Daten senden (Body)
*   **JSON**: Der Standard. Fügt automatisch `Content-Type: application/json` hinzu.
*   **Form-Data**: Wird für Datei-Uploads verwendet.
    *   *Parameter Type*: "File".
    *   *Input Data Field Name*: Der Name der Binäreigenschaft in n8n (Standard: `data`).
*   **Form URLencoded**: Wird von älteren APIs verwendet (z. B. Twilio). `application/x-www-form-urlencoded`.
*   **Raw**: Für XML, SOAP oder Klartext. Sie müssen den `Content-Type`-Header manuell setzen.

## 2. Antwortbehandlung & Parsing

### Antwortformat
*   **Autodetect**: n8n versucht, JSON zu parsen. Wenn es fehlschlägt, wird Text zurückgegeben.
*   **JSON**: Erzwingt JSON-Parsing. Fehler, wenn die Antwort HTML/Text ist.
*   **Binary**: **Entscheidend für Dateien**.
    *   *Response Data Property Name*: Wo die Datei gespeichert werden soll (z. B. `data`).
    *   Verwenden Sie dies beim Herunterladen von PDFs, Bildern oder CSVs.
*   **Text**: Gibt den rohen Body-String zurück.

### Fehlerbehandlungsoptionen
*   **Never Error**:
    *   **Aktiviert**: Der Node ist *immer* erfolgreich (grüner Ausgang).
    *   **Ausgabe**: Fügt ein `error`-Objekt zum Ausgabe-JSON hinzu, wenn der Status 4xx/5xx ist.
    *   **Anwendungsfall**: Wenn Sie 404s (Nicht gefunden) logisch in einem IF Node behandeln wollen (z. B. "Wenn 404, erstelle Benutzer; sonst aktualisiere Benutzer").
*   **Include Response Headers**:
    *   **Aktiviert**: Gibt Header wie `x-rate-limit-remaining` zurück.
    *   **Anwendungsfall**: Aufbau robuster Ratenbegrenzungslogik.

## 3. Paginierung (Looping)

Automatisiert das Abrufen großer Datensätze (z. B. "Hole alle 5000 Benutzer").

### Modus 1: Antwort enthält nächste URL
Die API gibt eine vollständige URL für die nächste Seite zurück (z. B. `next: "https://api.com?page=2"`).
*   **Next URL**: Ausdruck, der auf das Feld zeigt, z. B. `{{ $response.body.links.next }}`.
*   **Stop Condition**: Stoppt automatisch, wenn das Feld fehlt oder null ist.

### Modus 2: Parameter aktualisieren
Die API verwendet `page` oder `offset` Parameter.
*   **Typ**: "Offset" (0, 10, 20) oder "Page Number" (1, 2, 3).
*   **Parameter Name**: Der zu aktualisierende Query-Parameter (z. B. `page`).
*   **Increment**: Wie viel pro Schleife hinzugefügt werden soll (normalerweise `1` für Seiten, `limit` für Offset).
*   **Limit Parameter**: z. B. `limit=50`.
*   **Stop Condition**:
    *   *Empty List*: Stoppt, wenn die API ein leeres Array zurückgibt.
    *   *Field Value*: Stoppt, wenn `hasMore` falsch ist.

## 4. KI-Agenten-Integration (Tool Mode)

Wenn er mit einem **AI Agent** Node verbunden ist, verwandelt sich der HTTP Request Node in ein Tool.

### Tool-Einstellungen
*   **Name**: Der Funktionsname, den das LLM sieht (z. B. `get_weather`).
*   **Beschreibung**: **Kritisch**. Anweisungen für das LLM, *wann* und *wie* dieses Tool verwendet werden soll.
    *   *Schlecht*: "Holt Wetter."
    *   *Gut*: "Rufen Sie dies auf, um aktuelle Wetterdaten abzurufen. Erfordert einen 'city' Parameter (String)."

### Ausgabeoptimierung (Token sparen)
LLMs haben Kontextlimits. Das Ausgeben einer 5MB JSON-Antwort wird den Agenten zum Absturz bringen oder ein Vermögen kosten.
*   **Antwort optimieren**:
    *   **Fields to Extract**: Verwenden Sie JMESPath, um nur das Nötigste auszuwählen.
        *   Beispiel: `current_weather.temp_c, location.name`
    *   **Text Truncation**: Nach 2000 Zeichen abschneiden.
    *   **HTML to Text**: Wenn eine Website gescrapt wird, werden alle HTML-Tags entfernt, um sauberen Text zurückzugeben.

## 5. Fortgeschrittene Muster

### Batching (Vermeidung von Ratenlimits)
Wenn Sie 1000 Anfragen stellen müssen, das API-Limit aber 60/min beträgt:
1.  **Split In Batches**: Setzen Sie die Batch-Größe auf `1`.
2.  **HTTP Request**: Führen Sie den Aufruf durch.
3.  **Wait Node**: Setzen Sie auf `1` Sekunde.
4.  **Loop**: Verbinden Sie den Wait-Ausgang zurück zu Split In Batches.

### Proxy & Zertifikate
*   **Ignore SSL Issues**: Aktivieren Sie dies in "Options" für interne Tools mit selbstsignierten Zertifikaten.
*   **Proxy**: Konfigurieren Sie die `N8N_PROXY_RULES` Umgebungsvariable, um den Verkehr über einen Proxy zu leiten.

### cURL Import
Sie können einen rohen cURL-Befehl in den Node einfügen.
1.  Kopieren Sie cURL aus der API-Dokumentation.
2.  Klicken Sie im Node auf "Import cURL".
3.  Einfügen.
4.  n8n füllt automatisch Methode, URL, Header und Body aus.
