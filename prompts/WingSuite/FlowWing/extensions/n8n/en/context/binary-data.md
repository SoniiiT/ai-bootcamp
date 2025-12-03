---
description: Comprehensive guide for handling binary data (files, images, streams) in n8n.
globs: **/*.json
---

# Binary Data Handling Guide

This guide details how n8n manages files, images, and raw data streams, distinct from standard JSON data.

## Core Concepts

### JSON vs. Binary
Every item in n8n has two distinct data containers:
1.  **JSON (`$json`)**: Structured data (Strings, Numbers, Objects). Lightweight.
2.  **Binary (`$binary`)**: Raw file data. Heavyweight.

### Internal Storage Modes
n8n handles binary data differently based on the `N8N_DEFAULT_BINARY_DATA_MODE` environment variable:
*   **`default` (Memory)**: Binary data is kept in RAM as a Buffer. Fast but crashes the instance if files are too large.
*   **`filesystem` (Recommended for Production)**: Binary data is written to a temporary file on disk. Only a reference pointer is passed between nodes. Allows processing files larger than available RAM.

### The Binary Object
When you access `$binary.propertyName`, you get an object with metadata, not just the raw data.

```javascript
// Structure of $binary.data
{
  "data": "...",        // Base64 string (if in memory) or internal pointer
  "mimeType": "application/pdf",
  "fileName": "invoice.pdf",
  "fileExtension": "pdf",
  "fileSize": "1024"    // Size in bytes
}
```

---

## Working with Binary Data

### 1. Ingesting Files
*   **Read Binary File**: Loads a file from the local server's filesystem.
*   **HTTP Request**: Set **Response Format** to `File`.
*   **Webhook**: Accepts `multipart/form-data` uploads. The file appears in the Binary tab.
*   **Drive/S3/Dropbox**: Use "Download" operations.

### 2. Manipulating Files
*   **Move Binary Data**: The Swiss Army Knife.
    *   *Binary to JSON*: Extract text from a file (e.g., CSV to JSON array).
    *   *JSON to Binary*: Create a file from text (e.g., JSON array to CSV file).
*   **Compression**: Zip/Unzip files.
*   **Edit Image**: Resize, crop, or add text to images.
*   **Spreadsheet File**: Read/Write Excel (`.xlsx`) and CSV files.

### 3. Outputting Files
*   **Write Binary File**: Saves to the local server's filesystem.
*   **HTTP Request**: Set **Send Binary Data** to `true`.
*   **Drive/S3/Dropbox**: Use "Upload" operations.

---

## Scripting with Binary Data (Code Node)

Handling binary data in code requires understanding how n8n passes it.

### JavaScript (Node.js)

#### Reading Binary Data
```javascript
// Access the binary object
const binaryProperty = items[0].binary.data;

// Get the Base64 string (if available)
const base64String = binaryProperty.data;

// Convert to Buffer for processing
const buffer = Buffer.from(base64String, 'base64');

// Example: Get first 10 bytes
const header = buffer.subarray(0, 10);
```

#### Writing Binary Data
To create a new binary file from code, you must return the specific structure.

```javascript
const myString = "Hello World";
const buffer = Buffer.from(myString, 'utf8');

return [
  {
    json: { success: true },
    binary: {
      // Key name (e.g., 'data')
      data: {
        data: buffer.toString('base64'),
        mimeType: 'text/plain',
        fileName: 'hello.txt',
        fileExtension: 'txt'
      }
    }
  }
];
```

#### Helper Functions (n8n v1.0+)
n8n provides helpers to avoid manual Base64 conversion.
*   `getBinaryData()`: Returns a Buffer.
*   `setBinaryData()`: Sets binary data from a Buffer.

### Python

#### Reading Binary Data
```python
import base64

# Access binary data
binary_obj = items[0]['binary']['data']
base64_str = binary_obj['data']

# Decode to bytes
file_bytes = base64.b64decode(base64_str)

# Process bytes
header = file_bytes[:10]
```

---

## Common Patterns & Recipes

### 1. Creating a CSV File from JSON
Don't write a CSV string manually. Use the **Spreadsheet File** node.
1.  **Input**: Array of JSON items (e.g., 100 rows).
2.  **Node**: Spreadsheet File.
3.  **Operation**: "Write to File".
4.  **Output**: A single item with a binary property containing the `.csv` or `.xlsx` file.

### 2. Uploading a File via HTTP
1.  **Input**: Item with binary property `data`.
2.  **Node**: HTTP Request.
3.  **Method**: POST/PUT.
4.  **Send Binary Data**: Toggle `On`.
5.  **Binary Property**: `data`.
6.  **Multipart-Form-Data**: Toggle `On` if the API expects a form upload.

### 3. Dynamic Filenames
Often you want to rename a file based on JSON data.
1.  **Node**: Move Binary Data (or Code Node).
2.  **Mode**: "Update Binary Metadata" (if available) or just use the Code node to mutate `fileName`.

```javascript
// Code Node Example: Rename file based on ID
items[0].binary.data.fileName = `invoice_${items[0].json.id}.pdf`;
return items;
```

---

## Performance & Limits

### Memory Usage
*   **Problem**: Loading a 500MB video file into memory will likely crash the n8n worker if you have 1GB RAM.
*   **Solution**:
    1.  Use `N8N_DEFAULT_BINARY_DATA_MODE=filesystem`.
    2.  Avoid "splitting" binary items into thousands of JSON items if not needed.

### Data Pruning
Binary data is stored in the execution history.
*   **Impact**: High disk usage.
*   **Fix**:
    1.  Disable **Save Execution Progress** in workflow settings.
    2.  Set `EXECUTIONS_DATA_SAVE_ON_SUCCESS=false` globally.

### Max Payload Size
*   **Environment Variable**: `N8N_PAYLOAD_SIZE_MAX` (default ~16MB).
*   **Effect**: Limits the size of incoming HTTP requests (Webhooks).
*   **Fix**: Increase this value (e.g., `50` for 50MB) if you expect large uploads.

---

## Troubleshooting

### "Binary data property 'data' does not exist"
*   **Cause**: The previous node didn't output binary data, or it's named differently (e.g., `file` instead of `data`).
*   **Fix**: Check the "Binary" tab in the previous node's output. Use **Move Binary Data** to rename the key if needed.

### "Heap Out of Memory"
*   **Cause**: Processing large files in RAM.
*   **Fix**: Switch to `filesystem` mode or increase Node.js heap size (`NODE_OPTIONS="--max-old-space-size=4096"`).
