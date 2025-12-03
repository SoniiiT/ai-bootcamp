---
description: Umfassender Leitfaden zur Datentransformation, Datumsverarbeitung (Luxon) und eingebauten Funktionen in n8n.
applyTo: "**"
---

# Handbuch für Datentransformation & -manipulation

n8n bietet eine reiche Auswahl an Werkzeugen zur Datentransformation, von einfachen einzeiligen Ausdrücken bis hin zu komplexer JavaScript-Logik.

## 1. Datum & Zeit (Luxon)

n8n verwendet die [Luxon](https://moment.github.io/luxon/) Bibliothek für alle Datums- und Zeitoperationen. Sie ist global in Ausdrücken und Code Nodes verfügbar.

### Eingebaute Variablen
*   `$now`: Aktueller Zeitstempel als Luxon `DateTime` Objekt.
*   `$today`: Aktueller Zeitstempel abgerundet auf den Tagesbeginn (00:00:00).

### Häufige Operationen

#### Formatierung
Konvertieren eines Datumsobjekts in einen String.
*   **ISO 8601**: `{{ $now.toISO() }}` -> `2023-10-27T14:30:00.000Z`
*   **Benutzerdefiniertes Format**: `{{ $now.toFormat('yyyy-MM-dd HH:mm') }}` -> `2023-10-27 14:30`
*   **Menschenlesbar**: `{{ $now.toLocaleString() }}` -> `27.10.2023`

#### Parsing
Konvertieren eines Strings in ein Luxon-Objekt.
*   **Von ISO**: `{{ DateTime.fromISO('2023-10-27T14:30:00Z') }}`
*   **Von SQL**: `{{ DateTime.fromSQL('2023-10-27 14:30:00') }}`
*   **Von benutzerdefiniertem Format**: `{{ DateTime.fromFormat('27/10/2023', 'dd/MM/yyyy') }}`
*   **Von JS Date**: `{{ DateTime.fromJSDate(new Date()) }}`

#### Mathematik (Addieren/Subtrahieren)
*   **Addieren**: `{{ $now.plus({ days: 7, hours: 2 }) }}`
*   **Subtrahieren**: `{{ $now.minus({ months: 1 }) }}`
*   **Start von**: `{{ $now.startOf('month') }}` (Erster Tag des Monats)
*   **Ende von**: `{{ $now.endOf('week') }}` (Letzter Moment der Woche)

#### Zeitzonen
*   **Zone setzen**: `{{ $now.setZone('America/New_York') }}`
*   **Lokalzeit behalten**: `{{ $now.setZone('America/New_York', { keepLocalTime: true }) }}`

#### Differenz (Dauer)
Berechnen der Differenz zwischen zwei Daten.
```javascript
// Gibt die Differenz in Tagen zurück (z. B. 5.5)
{{ DateTime.fromISO($json.endDate).diff(DateTime.fromISO($json.startDate), 'days').days }}
```

---

## 2. String-Manipulation

Standard JavaScript String-Methoden sind verfügbar, plus n8n-Hilfsmethoden.

### Eingebaute Helfer
*   `.camelCase()`: `{{ 'my string'.camelCase() }}` -> `myString`
*   `.snakeCase()`: `{{ 'my string'.snakeCase() }}` -> `my_string`
*   `.kebabCase()`: `{{ 'My String'.kebabCase() }}` -> `my-string`
*   `.startCase()`: `{{ 'myString'.startCase() }}` -> `My String`
*   `.slugify()`: `{{ 'My String!'.slugify() }}` -> `my-string`
*   `.extractUrl()`: Gibt ein Array von URLs zurück, die im Text gefunden wurden.

### Standard JS Methoden
*   `.split(separator)`: `{{ 'a,b,c'.split(',') }}` -> `['a', 'b', 'c']`
*   `.replace(search, replacement)`: `{{ 'Hello World'.replace('World', 'n8n') }}`
*   `.trim()`: Entfernt Leerzeichen an beiden Enden.
*   `.toUpperCase()` / `.toLowerCase()`
*   `.includes(substring)`: Gibt true/false zurück.

---

## 3. Array- & Objekt-Manipulation

### Arrays
*   `.unique()`: Entfernt doppelte Werte aus einem Array.
*   `.random()`: Gibt ein zufälliges Element aus dem Array zurück.
*   `.join(separator)`: Verbindet Array-Elemente zu einem String.
*   `.map(callback)`: Transformiert jedes Element.
    *   `{{ $json.tags.map(tag => tag.name).join(', ') }}`
*   `.filter(callback)`: Filtert Elemente.
    *   `{{ $json.items.filter(item => item.price > 100) }}`
*   `.sort(callback)`: Sortiert das Array.

### Objekte
*   `Object.keys($json)`: Gibt ein Array der Schlüssel zurück.
*   `Object.values($json)`: Gibt ein Array der Werte zurück.
*   `JSON.stringify($json)`: Konvertiert Objekt in JSON-String.
*   `JSON.parse(string)`: Konvertiert JSON-String in Objekt.

---

## 4. JMESPath (JSON Abfragen)

n8n unterstützt [JMESPath](https://jmespath.org/) für komplexe JSON-Abfragen und Transformationen, ohne vollständiges JavaScript schreiben zu müssen.

*   **Verwendung**: `.jmespath('query')` Methode auf jedem Objekt.
*   **Beispiel**: Alle E-Mails aus einer Liste von Benutzern extrahieren, deren Alter > 18 ist.
    ```javascript
    {{ $json.users.jmespath("[?age > `18`].email") }}
    ```

### Häufige Muster
*   **Projektion**: `people[*].first_name` (Liste aller Vornamen abrufen)
*   **Filter**: `people[?state == 'WA']` (Leute in WA abrufen)
*   **Pipe**: `people[*].first_name | sort(@)` (Sortierte Namen abrufen)

---

## 5. Kryptographie & Hashing

Im **Code Node** können Sie das eingebaute Node.js `crypto` Modul verwenden.

```javascript
const crypto = require('crypto');

// MD5 Hash
const hash = crypto.createHash('md5').update('myString').digest('hex');

// SHA256 HMAC
const hmac = crypto.createHmac('sha256', 'secretKey').update('message').digest('hex');

// Random UUID
const uuid = crypto.randomUUID();
```

---

## 6. n8n Metadaten-Variablen

Zugriff auf System- und Workflow-Kontext.

| Variable | Beschreibung |
| :--- | :--- |
| `$execution.id` | Eindeutige ID der aktuellen Ausführung. |
| `$execution.mode` | `production` (Webhook/Trigger) oder `manual` (UI-Button). |
| `$workflow.id` | ID des aktuellen Workflows. |
| `$workflow.name` | Name des aktuellen Workflows. |
| `$env` | Zugriff auf Umgebungsvariablen (nur Self-hosted). |
| `$vars` | Zugriff auf [Custom Variables](https://docs.n8n.io/variables/), die in der UI definiert sind. |

---

## 7. Zugriff auf andere Nodes

Sie können Daten von *jedem* vorherigen Node referenzieren, nicht nur vom direkten Elternteil.

### Syntax
*   `$('Node Name')`: Gibt ein Selektor-Objekt für diesen Node zurück.

### Methoden
*   `.item`: Gibt das **gepaarte Item** zurück (das Item im referenzierten Node, das dem aktuell verarbeiteten Item entspricht). Dies ist die sicherste und häufigste Methode.
    *   `{{ $('Webhook').item.json.body.id }}`
*   `.first()`: Gibt das allererste Item des referenzierten Nodes zurück (Index 0).
    *   `{{ $('Webhook').first().json.body.id }}`
*   `.last()`: Gibt das letzte Item zurück.
*   `.all()`: Gibt ein Array *aller* Items von diesem Node zurück.
    *   `{{ $('Webhook').all()[0].json.body.id }}`

### Das "Paired Item" Konzept
n8n verfolgt automatisch die Abstammung. Wenn Sie 10 Items durch Ihren Workflow fließen lassen, weiß `$('Node A').item` genau, welches der 10 Items in "Node A" das aktuelle Item generiert hat, das Sie gerade verarbeiten.

---

## 8. Best Practices

1.  **Verwenden Sie Luxon**: Verwenden Sie immer Luxon für Daten. Vermeiden Sie das native `Date`, es sei denn, es ist für eine bestimmte Bibliothek erforderlich.
2.  **Fail Gracefully**: Verwenden Sie Optional Chaining (`?.`) in Ausdrücken, um Fehler zu vermeiden, wenn ein Feld fehlt.
    *   `{{ $json.user?.address?.city || 'Unbekannt' }}`
3.  **Komplexe Logik**: Wenn ein Ausdruck schwer zu lesen wird (verschachtelte Ternaries), verschieben Sie ihn in einen **Code Node**.
4.  **Zeitzonen**: Seien Sie immer explizit bei Zeitzonen. Server laufen normalerweise in UTC.
