---
description: Umfassender Leitfaden für den Umgang mit Binärdaten (Dateien, Bilder, Streams) in n8n.
globs: **/*.json
---

# Handbuch zur Verarbeitung von Binärdaten

Dieser Leitfaden beschreibt detailliert, wie n8n Dateien, Bilder und Rohdatenströme verwaltet, die sich von Standard-JSON-Daten unterscheiden.

## Kernkonzepte

### JSON vs. Binär
Jedes Item in n8n hat zwei unterschiedliche Datencontainer:
1.  **JSON (`$json`)**: Strukturierte Daten (Strings, Zahlen, Objekte). Leichtgewichtig.
2.  **Binär (`$binary`)**: Rohe Dateidaten. Schwergewichtig.

### Interne Speichermodi
n8n behandelt Binärdaten unterschiedlich, basierend auf der Umgebungsvariable `N8N_DEFAULT_BINARY_DATA_MODE`:
*   **`default` (Memory)**: Binärdaten werden als Buffer im RAM gehalten. Schnell, bringt aber die Instanz zum Absturz, wenn Dateien zu groß sind.
*   **`filesystem` (Empfohlen für Produktion)**: Binärdaten werden in eine temporäre Datei auf die Festplatte geschrieben. Nur ein Referenzzeiger wird zwischen Nodes übergeben. Ermöglicht die Verarbeitung von Dateien, die größer als der verfügbare RAM sind.

### Das Binärobjekt
Wenn Sie auf `$binary.propertyName` zugreifen, erhalten Sie ein Objekt mit Metadaten, nicht nur die Rohdaten.

```javascript
// Struktur von $binary.data
{
  "data": "...",        // Base64-String (wenn im Speicher) oder interner Zeiger
  "mimeType": "application/pdf",
  "fileName": "rechnung.pdf",
  "fileExtension": "pdf",
  "fileSize": "1024"    // Größe in Bytes
}
```

---

## Arbeiten mit Binärdaten

### 1. Dateien einlesen (Ingestion)
*   **Read Binary File**: Lädt eine Datei vom lokalen Dateisystem des Servers.
*   **HTTP Request**: Setzen Sie **Response Format** auf `File`.
*   **Webhook**: Akzeptiert `multipart/form-data` Uploads. Die Datei erscheint im Binary-Tab.
*   **Drive/S3/Dropbox**: Verwenden Sie "Download"-Operationen.

### 2. Dateien manipulieren
*   **Move Binary Data**: Das Schweizer Taschenmesser.
    *   *Binary to JSON*: Text aus einer Datei extrahieren (z. B. CSV zu JSON-Array).
    *   *JSON to Binary*: Eine Datei aus Text erstellen (z. B. JSON-Array zu CSV-Datei).
*   **Compression**: Dateien zippen/entpacken.
*   **Edit Image**: Bilder skalieren, zuschneiden oder Text hinzufügen.
*   **Spreadsheet File**: Excel (`.xlsx`) und CSV-Dateien lesen/schreiben.

### 3. Dateien ausgeben
*   **Write Binary File**: Speichert auf dem lokalen Dateisystem des Servers.
*   **HTTP Request**: Setzen Sie **Send Binary Data** auf `true`.
*   **Drive/S3/Dropbox**: Verwenden Sie "Upload"-Operationen.

---

## Scripting mit Binärdaten (Code Node)

Der Umgang mit Binärdaten im Code erfordert Verständnis dafür, wie n8n diese übergibt.

### JavaScript (Node.js)

#### Binärdaten lesen
```javascript
// Zugriff auf das Binärobjekt
const binaryProperty = items[0].binary.data;

// Den Base64-String abrufen (falls verfügbar)
const base64String = binaryProperty.data;

// Zur Verarbeitung in Buffer konvertieren
const buffer = Buffer.from(base64String, 'base64');

// Beispiel: Die ersten 10 Bytes abrufen
const header = buffer.subarray(0, 10);
```

#### Binärdaten schreiben
Um eine neue Binärdatei aus Code zu erstellen, müssen Sie die spezifische Struktur zurückgeben.

```javascript
const myString = "Hallo Welt";
const buffer = Buffer.from(myString, 'utf8');

return [
  {
    json: { success: true },
    binary: {
      // Schlüsselname (z. B. 'data')
      data: {
        data: buffer.toString('base64'),
        mimeType: 'text/plain',
        fileName: 'hallo.txt',
        fileExtension: 'txt'
      }
    }
  }
];
```

#### Hilfsfunktionen (n8n v1.0+)
n8n bietet Helfer, um manuelle Base64-Konvertierung zu vermeiden.
*   `getBinaryData()`: Gibt einen Buffer zurück.
*   `setBinaryData()`: Setzt Binärdaten aus einem Buffer.

### Python

#### Binärdaten lesen
```python
import base64

# Zugriff auf Binärdaten
binary_obj = items[0]['binary']['data']
base64_str = binary_obj['data']

# In Bytes dekodieren
file_bytes = base64.b64decode(base64_str)

# Bytes verarbeiten
header = file_bytes[:10]
```

---

## Häufige Muster & Rezepte

### 1. Erstellen einer CSV-Datei aus JSON
Schreiben Sie keinen CSV-String manuell. Verwenden Sie den **Spreadsheet File** Node.
1.  **Eingabe**: Array von JSON-Items (z. B. 100 Zeilen).
2.  **Node**: Spreadsheet File.
3.  **Operation**: "Write to File".
4.  **Ausgabe**: Ein einzelnes Item mit einer Binäreigenschaft, die die `.csv` oder `.xlsx` Datei enthält.

### 2. Hochladen einer Datei via HTTP
1.  **Eingabe**: Item mit Binäreigenschaft `data`.
2.  **Node**: HTTP Request.
3.  **Methode**: POST/PUT.
4.  **Send Binary Data**: Schalten Sie auf `On`.
5.  **Binary Property**: `data`.
6.  **Multipart-Form-Data**: Schalten Sie auf `On`, wenn die API einen Formular-Upload erwartet.

### 3. Dynamische Dateinamen
Oft möchten Sie eine Datei basierend auf JSON-Daten umbenennen.
1.  **Node**: Move Binary Data (oder Code Node).
2.  **Modus**: "Update Binary Metadata" (falls verfügbar) oder verwenden Sie einfach den Code Node, um `fileName` zu ändern.

```javascript
// Code Node Beispiel: Datei basierend auf ID umbenennen
items[0].binary.data.fileName = `rechnung_${items[0].json.id}.pdf`;
return items;
```

---

## Leistung & Limits

### Speichernutzung
*   **Problem**: Das Laden einer 500MB Videodatei in den Speicher wird wahrscheinlich den n8n-Worker zum Absturz bringen, wenn Sie 1GB RAM haben.
*   **Lösung**:
    1.  Verwenden Sie `N8N_DEFAULT_BINARY_DATA_MODE=filesystem`.
    2.  Vermeiden Sie das "Aufteilen" von Binär-Items in tausende von JSON-Items, wenn nicht nötig.

### Datenbereinigung (Pruning)
Binärdaten werden im Ausführungsverlauf gespeichert.
*   **Auswirkung**: Hohe Festplattennutzung.
*   **Lösung**:
    1.  Deaktivieren Sie **Save Execution Progress** in den Workflow-Einstellungen.
    2.  Setzen Sie `EXECUTIONS_DATA_SAVE_ON_SUCCESS=false` global.

### Maximale Payload-Größe
*   **Umgebungsvariable**: `N8N_PAYLOAD_SIZE_MAX` (Standard ~16MB).
*   **Effekt**: Begrenzt die Größe eingehender HTTP-Anfragen (Webhooks).
*   **Lösung**: Erhöhen Sie diesen Wert (z. B. `50` für 50MB), wenn Sie große Uploads erwarten.

---

## Fehlerbehebung

### "Binary data property 'data' does not exist"
*   **Ursache**: Der vorherige Node hat keine Binärdaten ausgegeben, oder sie heißen anders (z. B. `file` statt `data`).
*   **Lösung**: Überprüfen Sie den "Binary"-Tab in der Ausgabe des vorherigen Nodes. Verwenden Sie **Move Binary Data**, um den Schlüssel bei Bedarf umzubenennen.

### "Heap Out of Memory"
*   **Ursache**: Verarbeitung großer Dateien im RAM.
*   **Lösung**: Wechseln Sie in den `filesystem`-Modus oder erhöhen Sie die Node.js Heap-Größe (`NODE_OPTIONS="--max-old-space-size=4096"`).
