---
description: Umfassender Leitfaden zur Verwendung von Ausdrücken (Expressions) in n8n für dynamisches Daten-Mapping.
applyTo: "**"
---

# Handbuch für Ausdrücke (Expressions)

Ausdrücke sind der Klebstoff, der n8n-Workflows zusammenhält. Sie ermöglichen es Ihnen, Daten dynamisch von einem Node zu einem anderen unter Verwendung von JavaScript abzubilden.

## Kernsyntax

Ausdrücke werden in doppelte geschweifte Klammern verpackt: `{{ ... }}`.
Innerhalb der Klammern können Sie gültigen JavaScript-Code schreiben.

*   **Einfache Variable**: `{{ $json.myField }}`
*   **Verschachteltes Feld**: `{{ $json.address.city }}`
*   **Array-Zugriff**: `{{ $json.tags[0] }}`
*   **Mathematik**: `{{ $json.price * $json.quantity }}`
*   **String-Verkettung**: `{{ "Hallo " + $json.firstName }}`
*   **Template Literals**: `{{ `Hallo ${$json.firstName}` }}`

---

## Datenzugriffsvariablen

Diese globalen Objekte geben Ihnen Zugriff auf den Datenkontext des Workflows.

### 1. `$json` (Aktuelles Item)
Greift auf die JSON-Nutzlast des *aktuellen Items* zu, das verarbeitet wird.
*   **Verwendung**: Häufigste Variable.
*   **Beispiel**: `{{ $json.email }}`

### 2. `$binary` (Binärdaten)
Greift auf Metadaten von Binärdateien zu, die an das Item angehängt sind.
*   **Verwendung**: Abrufen von Dateinamen, MIME-Typen.
*   **Beispiel**: `{{ $binary.data.fileName }}`

### 3. `$node` (Vorherige Nodes)
Greift auf Daten von *jedem* vorherigen Node im Workflow zu.
*   **Syntax**: `$node["Node Name"].json.field`
*   **Kontext**: Löst automatisch auf das "Paired Item" auf (siehe unten).
*   **Beispiel**: `{{ $node["Webhook"].json.body.id }}`

### 4. `$env` (Umgebung)
Greift auf Umgebungsvariablen zu (nur Self-hosted).
*   **Verwendung**: API-Schlüssel, Konfigurations-Flags.
*   **Beispiel**: `{{ $env.MY_API_KEY }}`

### 5. `$vars` (Benutzerdefinierte Variablen)
Greift auf Variablen zu, die in **Settings > Variables** definiert sind.
*   **Verwendung**: Globale Konstanten.
*   **Beispiel**: `{{ $vars.SLACK_CHANNEL_ID }}`

### 6. `$now` / `$today`
Luxon DateTime Objekte für die aktuelle Zeit.
*   **Beispiel**: `{{ $now.minus({ days: 1 }).toISO() }}`

---

## Das "Paired Item" Konzept

Dies zu verstehen ist **entscheidend** für Schleifen und das Zusammenführen von Daten.

Wenn Node C Node A referenziert (und Node B überspringt), muss n8n wissen: "Für das Item, das ich gerade in Node C verarbeite, welchem Item in Node A entspricht es?"

*   **Automatische Auflösung**: `{{ $node["Node A"].json.id }}`
    *   n8n findet automatisch das Vorfahren-Item. Dies funktioniert in 99% der Fälle.
*   **Manuelle Auflösung**: `{{ $('Node A').item.json.id }}`
    *   Fragt explizit nach dem gepaarten Item.
*   **Erstes Item**: `{{ $('Node A').first().json.id }}`
    *   Ignoriert die Paarung und greift immer das erste Item von Node A. Nützlich für "Globale" Konfigurations-Nodes, die nur 1 Item ausgeben.
*   **Alle Items**: `{{ $('Node A').all() }}`
    *   Gibt ein Array aller Items zurück. Nützlich für Aggregation.

---

## Fortgeschrittene Logik

Da Ausdrücke JavaScript sind, können Sie komplexe Logik ausführen.

### Ternärer Operator (If/Else)
```javascript
{{ $json.active ? "Aktiver Benutzer" : "Inaktiver Benutzer" }}
```

### Kurzschlussauswertung (Short-circuit Evaluation)
```javascript
// Standardwert verwenden, wenn Name fehlt
{{ $json.name || "Anonym" }}

// Sicherer Zugriff (Optional Chaining)
{{ $json.user?.profile?.email }}
```

### Array-Operationen
```javascript
// Tags mit Komma verbinden
{{ $json.tags.join(', ') }}

// Summe eines Arrays von Zahlen
{{ $json.prices.reduce((a, b) => a + b, 0) }}
```

### IIFE (Immediately Invoked Function Expression)
Wenn Sie mehrere Zeilen Logik oder temporäre Variablen benötigen, verpacken Sie es in eine Funktion.

```javascript
{{
  (() => {
    const firstName = $json.name.split(' ')[0];
    const domain = $json.email.split('@')[1];
    if (domain === 'gmail.com') {
      return `Privat: ${firstName}`;
    }
    return `Geschäftlich: ${firstName}`;
  })()
}}
```

---

## Debugging von Ausdrücken

### 1. Das Vorschaufenster (Preview Pane)
Wenn Sie einen Ausdruck in der UI eingeben, zeigt der "Expression"-Tab das **Ergebnis** sofort basierend auf dem ersten Item der Eingabedaten an.
*   **Tipp**: Wenn dort `[Object object]` steht, verpacken Sie es in `JSON.stringify(...)`, um die Struktur zu sehen.

### 2. `[Object object]` Fehler
Dies passiert, wenn Sie versuchen, ein JS-Objekt in ein Feld einzufügen, das einen String erwartet.
*   **Lösung**: Greifen Sie auf eine bestimmte Eigenschaft zu (`$json.myObj.id`) oder stringifizieren Sie es (`JSON.stringify($json.myObj)`).

### 3. "Referenced node is unexecuted"
Sie versuchen, Daten von einem Node zu referenzieren, der noch nicht gelaufen ist (oder von einem IF Node übersprungen wurde).
*   **Lösung**: Stellen Sie sicher, dass der Node verbunden ist und ausgeführt wurde. Verwenden Sie Optional Chaining (`?.`), wenn der Node übersprungen werden könnte.

---

## Best Practices

1.  **Halten Sie es einfach**: Wenn Ihr Ausdruck mehr als 3 Zeilen hat, ziehen Sie einen **Code Node** in Betracht. Er ist einfacher zu lesen, zu debuggen und versionszukontrollieren.
2.  **Verwenden Sie `$json`**: Bevorzugen Sie `$json` gegenüber `$node["Previous Node"].json` wann immer möglich. Es macht den Node portabel (Sie können ihn kopieren und einfügen, ohne Referenzen zu brechen).
3.  **Typsicherheit**: Denken Sie daran, dass Eingabefelder in n8n typisiert sind.
    *   Wenn ein Feld eine **Zahl** erwartet, muss Ihr Ausdruck eine Zahl zurückgeben (z. B. `{{ 123 }}`), keinen String (`{{ "123" }}`).
    *   Wenn ein Feld einen **Boolean** erwartet, geben Sie `true`/`false` zurück.
4.  **Kommentare**: Sie können Kommentare innerhalb von Ausdrücken mit der Standard-JS-Syntax `//` oder `/* */` hinzufügen.

```javascript
{{
  // Steuer berechnen
  $json.price * 0.2
}}
```
