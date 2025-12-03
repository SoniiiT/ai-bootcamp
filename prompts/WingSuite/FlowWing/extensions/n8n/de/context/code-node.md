# n8n Code Node Handbuch

## Übersicht
Der Code Node ist der ultimative Notausgang in n8n. Er ermöglicht es Ihnen, benutzerdefinierte JavaScript- (Node.js) oder Python-Logik auszuführen, wenn eingebaute Nodes eine spezifische Transformations- oder Logikanforderung nicht erfüllen können.

## 1. Ausführungsmodi

### Run Once for All Items (Empfohlen)
*   **Verhalten**: Der Code wird **genau einmal** ausgeführt, unabhängig davon, wie viele Items in den Node gelangen.
*   **Eingabe**: Sie erhalten ein Array aller Items (`items` oder `$input.all()`).
*   **Ausgabe**: Sie müssen ein Array von Objekten zurückgeben.
*   **Anwendungsfälle**:
    *   Aggregation (Summe, Durchschnitt).
    *   Sortieren / Filtern basierend auf dem gesamten Datensatz.
    *   Pivotieren von Daten (Array zu Objekt).
    *   Stapelverarbeitung (Batch Processing).

### Run Once for Each Item
*   **Verhalten**: Der Code wird **N-mal** ausgeführt, wobei N die Anzahl der Eingabe-Items ist.
*   **Eingabe**: Sie erhalten ein einzelnes Item (`item` oder `$input.item`).
*   **Ausgabe**: Sie geben ein einzelnes Objekt zurück (oder `undefined`, um es herauszufiltern).
*   **Anwendungsfälle**:
    *   Einfache 1:1 Transformationen (z. B. Regex auf einem String).
    *   Berechnungen pro Zeile.
*   **Leistungswarnung**: Langsamer bei großen Datensätzen (1000+ Items) aufgrund des Overheads beim N-maligen Aufrufen der Sandbox.

## 2. JavaScript (Node.js)

### Datenstruktur
n8n verwendet eine spezifische Struktur. Jedes Item ist ein Objekt mit zwei Schlüsseln:
1.  `json`: Die eigentlichen Daten (Objekt).
2.  `binary`: Binäre Dateidaten (Objekt, optional).

**Korrektes Rückgabeformat:**
```javascript
return [
  {
    json: {
      id: 1,
      name: "Alice"
    },
    binary: {
      image: {
        data: "base64string...",
        mimeType: "image/png",
        fileName: "alice.png"
      }
    }
  }
];
```

### Eingebaute Bibliotheken & Globale Variablen
*   `$input`: Zugriff auf Eingabedaten.
    *   `$input.all()`: Alle Items.
    *   `$input.first()`: Erstes Item.
    *   `$input.last()`: Letztes Item.
    *   `$input.item`: Aktuelles Item (im "Run for Each" Modus).
*   `$node`: Zugriff auf Node-Metadaten.
*   `$env`: Zugriff auf Umgebungsvariablen (nur Self-hosted).
*   `DateTime`: [Luxon](https://moment.github.io/luxon/) Bibliothek für Datumsmanipulation.
*   `require()`:
    *   **Cloud**: Beschränkt auf `crypto`, `lodash`, `moment`.
    *   **Self-hosted**: Kann jedes npm-Paket importieren, wenn `NODE_FUNCTION_ALLOW_EXTERNAL` in `.env` gesetzt ist.

### Häufige Snippets

#### Daten von zwei vorherigen Nodes manuell zusammenführen
```javascript
const users = $("Get Users").all();
const orders = $("Get Orders").all();

// Join über userId
return orders.map(order => {
  const user = users.find(u => u.json.id === order.json.userId);
  return {
    json: {
      ...order.json,
      userName: user ? user.json.name : "Unknown"
    }
  };
});
```

#### Verschachtelte Arrays flachklopfen (Flattening)
Wenn eine API `{ "users": [...] }` zurückgibt und Sie ein Item pro Benutzer wünschen:
```javascript
// Eingabe: [{ json: { users: [ {id:1}, {id:2} ] } }]
const users = items[0].json.users;

return users.map(user => {
  return {
    json: user
  };
});
```

#### Umgang mit Binärdaten
Erstellen einer Textdatei aus einem String:
```javascript
const myString = "Hallo Welt";
const buffer = Buffer.from(myString, 'utf8');

return [
  {
    json: { success: true },
    binary: {
      data: {
        data: buffer.toString('base64'),
        mimeType: 'text/plain',
        fileName: 'hallo.txt'
      }
    }
  }
];
```

## 3. Python (Beta)

### Konfiguration
*   **Python-Version**: Hängt von der Umgebung ab (normalerweise Python 3).
*   **Bibliotheken**: Standardbibliothek ist verfügbar. Externe `pip`-Pakete müssen im n8n-Container/Umgebung installiert sein.

### Syntax-Unterschiede
*   **Eingabe**: `_input.all()` gibt eine Liste von Dictionaries zurück.
*   **Ausgabe**: Geben Sie eine Liste von Dictionaries zurück. n8n verpackt diese automatisch in die `json`-Schlüsselstruktur.

```python
# Python Beispiel
items = _input.all()
output = []

for item in items:
    # Zugriff auf JSON-Daten direkt
    data = item.json
    
    # Logik
    data['new_field'] = data['value'] * 2
    
    output.append(item)

return output
```

## 4. Debugging von Code Nodes
*   **console.log()**: Die Ausgabe wird in der **Browser-Konsole** (F12) angezeigt, NICHT in der n8n-UI-Ausgabe.
*   **Browser-Konsole**: Öffnen Sie die DevTools -> Konsole, um Ihre Logs zu sehen.
*   **Fehler werfen**: `throw new Error("Mein benutzerdefinierter Fehler")` stoppt den Workflow und zeigt die Nachricht in der UI an.

## 5. Best Practices
1.  **Immer Arrays zurückgeben**: Auch wenn Sie ein einzelnes Item ausgeben, verpacken Sie es in `[]`.
2.  **Binärdaten bewahren**: Wenn Sie über Items mappen, stellen Sie sicher, dass Sie den `binary`-Schlüssel nicht verlieren, wenn Sie ihn später benötigen.
    *   *Schlecht*: `return items.map(i => ({ json: { ...i.json, new: 1 } }))` (Verliert Binärdaten)
    *   *Gut*: `return items.map(i => ({ ...i, json: { ...i.json, new: 1 } }))` (Behält Binärdaten)
3.  **Speichernutzung**: Vermeiden Sie das Erstellen riesiger Arrays im Speicher, wenn Sie Millionen von Zeilen verarbeiten. Verwenden Sie bei Bedarf `Split In Batches` vor dem Code Node.
