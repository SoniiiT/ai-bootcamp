---
description: Umfassender Leitfaden zur n8n-Plattformarchitektur, Datenstrukturen und Ausführungslogik.
globs: **/*.json
---

# n8n Plattform-Architektur-Handbuch

Zu verstehen, wie n8n denkt, ist der Schlüssel zum Aufbau robuster Workflows. Dieser Leitfaden behandelt die interne Datenstruktur, den Ausführungsfluss und das Zustandsmanagement.

## 1. Die Datenstruktur

n8n hat eine feste Meinung zu Daten. Jeder Node empfängt und gibt Daten in einem bestimmten Format aus.

### Das "Item"
Die grundlegende Dateneinheit ist ein **Item**. Ein Item ist ein Objekt mit zwei Haupteigenschaften:
1.  **`json`**: Ein Standard-JavaScript-Objekt, das Ihre Datenfelder enthält.
2.  **`binary`**: Ein Objekt, das Binärdateireferenzen enthält (optional).

### Das "Input Array"
Nodes verarbeiten keine einzelnen Items; sie verarbeiten ein **Array von Items**.
Selbst wenn Sie nur ein Datenstück haben, ist es in ein Array verpackt.

```javascript
[
  {
    "json": {
      "id": 1,
      "name": "Alice"
    },
    "binary": {}
  },
  {
    "json": {
      "id": 2,
      "name": "Bob"
    },
    "binary": {}
  }
]
```

### Implikation: Implizites Looping
Da die Eingabe immer ein Array ist, **loopen die meisten Nodes automatisch**.
*   Wenn Sie einen "Google Sheets Read" (der 100 Zeilen zurückgibt) mit einem "Slack Send Message" Node verbinden, wird der Slack Node **100 Mal** ausgeführt, einmal für jedes Item.
*   **Sie benötigen keinen Loop Node** für Standard-1-zu-1-Verarbeitung.

---

## 2. Ausführungslogik

### Node-Ausführung
n8n führt Nodes in einer topologischen Reihenfolge (Abhängigkeitsgraph) aus.
1.  **Start**: Der Trigger Node läuft und produziert ein Array von Items.
2.  **Next**: Der nächste Node empfängt dieses Array.
3.  **Processing**: Der Node verarbeitet die Items (entweder alle auf einmal oder eins nach dem anderen, abhängig vom Node-Typ).
4.  **Output**: Der Node gibt ein neues Array von Items für den nächsten Node aus.

### Verzweigung (IF / Switch)
*   **Splitting**: Wenn ein `IF` Node läuft, teilt er das Eingabe-Array in zwei Arrays: `True` und `False`.
*   **Parallele Ausführung**: Beide Zweige können ausgeführt werden, wenn Items beide Pfade durchlaufen.

### Zusammenführen (Merging)
Um Zweige wieder zusammenzuführen, **müssen** Sie einen **Merge** Node verwenden.
*   **Append**: Stapelt einfach Items von Input 1 und Input 2 in ein einziges Array.
*   **Wait (Pass-through)**: Wartet, bis Input 1 UND Input 2 fertig sind, und gibt dann Input 1 aus (ignoriert die Daten von Input 2, nutzt sie aber als Signal).
*   **Keep Key Matches**: Verbindet Items wie ein SQL `JOIN` basierend auf einem gemeinsamen Feld (z. B. `email`).
*   **Multiplex**: Erstellt ein kartesisches Produkt (jedes Item von Input 1 kombiniert mit jedem Item von Input 2).

---

## 3. Looping (Explizit)

Während implizites Looping 1-zu-1-Aufgaben handhabt, benötigen Sie den **Loop Over Items** Node für:
1.  **Batching**: Senden von 10 Items auf einmal an eine API, um Ratenlimits einzuhalten.
2.  **Resetting Context**: Wenn Sie eine Summe *pro Item* unter Verwendung eines Sub-Workflows berechnen müssen.
3.  **Fan-out**: Wenn ein Item mehrere Ausgabe-Items generieren muss, die eine unterschiedliche Verarbeitung benötigen.

### Konfiguration
*   **Batch Size**: Wie viele Items in einer Iteration verarbeitet werden sollen (Standard 1).
*   **Loop End**: Sie müssen das Ende der Schleife zurück zum **Loop Over Items** Node verbinden, um den Abschluss eines Batches zu signalisieren.

---

## 4. Zustandsmanagement

### Workflow Static Data
n8n ist standardmäßig zustandslos. Um Daten *zwischen* Ausführungen zu speichern (z. B. "Zuletzt abgefragte ID"), verwenden Sie **Static Data**.

*   **Scope**:
    *   `global`: Geteilt über den gesamten Workflow.
    *   `node`: Auf einen spezifischen Node beschränkt (verwendet von Polling-Triggern).
*   **Verwendung (Code Node)**:
    ```javascript
    const staticData = getWorkflowStaticData('global');
    const lastId = staticData.lastId || 0;
    
    // Update logic
    staticData.lastId = 123;
    ```
*   **Wichtig**: Statische Daten werden **nur gespeichert**, wenn der Workflow im **Production**-Modus läuft (über einen Trigger). Sie werden NICHT während manueller Tests in der UI gespeichert.

### Variablen (`$vars`)
*   Definiert in **Settings > Variables**.
*   Schreibgeschützt während der Ausführung.
*   Gut für Konstanten wie `API_BASE_URL` oder `SLACK_CHANNEL_ID`.

---

## 5. KI-Architektur (LangChain)

Die KI-Implementierung von n8n unterscheidet sich von Standard-Nodes.

### Das "Cluster"-Konzept
*   **Root Node**: Der **AI Agent** oder die **Chain**. Dies ist der einzige Node, der im Standardfluss "ausgeführt" wird.
*   **Sub-Nodes**: Nodes wie **Chat Model**, **Memory** und **Tools** verbinden sich mit dem Root Node über *spezielle Eingänge* (graue Punkte), nicht über den Hauptausgang.
*   **Ausführung**: Wenn der Agent läuft, "zieht" er Fähigkeiten von den Sub-Nodes. Die Sub-Nodes laufen nicht unabhängig; sie sind Konfigurationsanbieter für den Agenten.

---

## 6. Best Practices

1.  **Loops minimieren**: Explizite Loops sind langsam. Verwenden Sie implizites Looping (Batch-Verarbeitung) wann immer möglich.
2.  **Früh filtern**: Verwenden Sie `IF` Nodes frühzeitig, um unnötige Items zu entfernen und Verarbeitungszeit zu sparen.
3.  **Korrekt mergen**: Wenn Sie einen Fluss aufteilen, führen Sie ihn immer mit einem Merge Node zusammen, wenn Sie die Daten später benötigen. Offene Zweige können unerwartetes Verhalten verursachen.
4.  **Benennung**: Benennen Sie Nodes sofort um. `HTTP Request 15` ist unmöglich zu debuggen. Verwenden Sie `Get User Data`.
