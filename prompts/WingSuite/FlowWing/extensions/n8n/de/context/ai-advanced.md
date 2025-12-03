# Fortgeschrittener KI & LangChain Leitfaden

Dieser Leitfaden bietet einen tiefen Einblick in den Aufbau autonomer Agenten, RAG-Pipelines und komplexer KI-Workflows unter Verwendung der LangChain-Integration von n8n.

## Kernarchitektur

Die KI-Fähigkeiten von n8n basieren auf **LangChain**. Die Architektur unterscheidet sich von Standard-n8n-Workflows:
*   **Haupt-Node**: Das "Gehirn" (Agent oder Chain), das die Logik ausführt.
*   **Sub-Nodes**: "Gliedmaßen", die Fähigkeiten bereitstellen (Modelle, Speicher, Tools, Retriever). Diese verbinden sich mit spezifischen *Eingabe-Ports* am Haupt-Node, nicht mit dem Standard-Workflow-Ausgang.

---

## KI-Agenten

Agenten verwenden ein LLM, um darüber nachzudenken, *was* zu tun ist. Sie können Tools aufrufen, die Ausgabe beobachten und den nächsten Schritt entscheiden.

### Agenten-Typen

| Agenten-Typ | Am besten für | Beschreibung |
| :--- | :--- | :--- |
| **Tools Agent** | Allgemeine Zwecke | Der vielseitigste Agent. Verwendet OpenAI's Function Calling (oder ähnlich), um Tools zuverlässig auszuführen. Empfohlen für die meisten Anwendungsfälle. |
| **Conversational Agent** | Chatbots | Optimiert für die Aufrechterhaltung des Gesprächsverlaufs. Weniger zuverlässig bei komplexer Tool-Nutzung als der Tools Agent. |
| **ReAct Agent** | Non-Function Modelle | Verwendet das "Reasoning + Acting" Muster. Gut für Modelle, die kein natives Function Calling unterstützen (z. B. ältere Open-Source-Modelle). Ausführlicher und langsamer. |
| **Plan & Execute** | Komplexe Aufgaben | Zerlegt ein komplexes Ziel in einen Schritt-für-Schritt-Plan und führt dann jeden Schritt aus. Gut für mehrstufige Recherchen. |

### Konfiguration im Detail

#### 1. System Prompt
Die wichtigste Konfiguration. Definieren Sie:
*   **Persona**: "Du bist ein Senior Support Engineer..."
*   **Einschränkungen**: "Erfinde niemals Fakten. Wenn du es nicht weißt, sag es."
*   **Formatierung**: "Antworte immer in Markdown."
*   **Datum/Zeit**: Injizieren Sie `{{ $now }}`, damit der Agent den aktuellen Kontext kennt.

#### 2. Speicher (Memory / Context)
*   **Window Buffer Memory**: Behält die letzten K Interaktionen. Am besten für die meisten Chatbots.
*   **Summary Buffer Memory**: Fasst ältere Interaktionen zusammen, um Token zu sparen, während die jüngsten wörtlich bleiben.
*   **Vector Store Memory**: Speichert den Gesprächsverlauf in einer Vektordatenbank für langfristige Erinnerung (unendlicher Speicher).
*   **Sitzungsverwaltung**: Verwenden Sie die Eigenschaft `sessionId` im Memory-Node, um Gespräche zwischen Benutzern zu isolieren.
    *   *Muster*: Setzen Sie `Session ID` auf `{{ $json.chatId }}` (kommend von einem Webhook oder Chat-Trigger).

---

## Tools & Fähigkeiten

Tools ermöglichen es dem Agenten, mit der Außenwelt zu interagieren.

### Eingebaute Tools
*   **Calculator**: Mathematische Operationen.
*   **Wikipedia / SerpApi**: Websuche und Wissen.
*   **Code Tool**: Führt Python/JS-Code in einer Sandbox aus.

### Benutzerdefinierte Tools (Das Kraftpaket)
Sie können *jeden* n8n-Workflow oder HTTP-Request in ein Tool verwandeln.

#### 1. Call n8n Workflow Tool
Delegiert eine Aufgabe an einen anderen Workflow.
*   **Name**: Muss beschreibend sein (z. B. `get_customer_order`).
*   **Beschreibung**: **KRITISCH**. Das LLM liest dies, um zu entscheiden, *wann* das Tool verwendet werden soll. Beispiel: "Rufen Sie dies auf, um Bestelldetails anhand einer Bestell-ID abzurufen."
*   **Schema**: Definieren Sie die Eingaben, die das Tool erwartet (z. B. `orderId` als String).
*   **Workflow**: Der Sub-Workflow muss mit einem `Execute Workflow Trigger` beginnen und mit einem `Respond to Webhook` Node enden (der die Tool-Ausgabe zurückgibt).

#### 2. HTTP Request Tool
Ruft direkt eine API auf.
*   **Einrichtung**: Ähnlich wie der Standard HTTP Request Node, aber vom Agenten ausgelöst.
*   **Dynamische Parameter**: Verwenden Sie Ausdrücke, um Tool-Eingaben auf API-Parameter abzubilden.

#### 3. Dynamische Tool-Eingaben (`$fromAI()`)
Wenn Sie Tool-Parameter definieren, möchten Sie oft, dass das LLM den Wert liefert.
*   **Verwendung**: Setzen Sie in der Tool-Konfiguration einen Parameter auf **Expression** und verwenden Sie `$fromAI('parameterName')`.
*   **Beschreibung**: Sie müssen eine Beschreibung für `parameterName` angeben, damit das LLM weiß, was dort eingetragen werden soll.

---

## RAG (Retrieval Augmented Generation)

RAG ermöglicht es der KI, Fragen basierend auf Ihren eigenen Daten (PDFs, Notion, SQL, usw.) zu beantworten, ohne Fine-Tuning.

### Die RAG-Pipeline

#### Phase 1: Ingestion (Daten laden)
1.  **Document Loader**: Liest Daten.
    *   *Nodes*: `Text File`, `PDF Loader`, `Notion`, `Web Scraper`, `Github`.
2.  **Text Splitter**: Zerlegt Daten in handhabbare Stücke (Chunks).
    *   *Node*: `Recursive Character Text Splitter`.
    *   *Konfig*: `Chunk Size` (z. B. 1000 Zeichen), `Chunk Overlap` (z. B. 200 Zeichen). Überlappung ist entscheidend, um den Kontext zwischen Chunks zu erhalten.
3.  **Embeddings**: Konvertiert Text in Vektoren.
    *   *Node*: `OpenAI Embeddings`, `HuggingFace Embeddings`.
4.  **Vector Store (Insert)**: Speichert Vektoren.
    *   *Nodes*: `Pinecone`, `Qdrant`, `Supabase`, `Postgres (PgVector)`.
    *   *Modus*: Wählen Sie **Insert Documents**.

#### Phase 2: Retrieval (Abfrage)
1.  **Vector Store (Retrieve)**: Findet relevante Chunks.
    *   *Modus*: Wählen Sie **Retrieve Documents** (oder verbinden Sie es als Retriever mit einer Chain).
    *   *Query*: Kommt normalerweise aus der Frage des Benutzers.
2.  **QA Chain**: Synthetisiert die Antwort.
    *   *Node*: `Conversational Retrieval QA Chain`.
    *   *Eingaben*: Verbinden Sie das **Chat Model** und den **Vector Store Retriever**.

### Fortgeschrittene RAG-Techniken
*   **Hybrid Search**: Kombiniert Stichwortsuche (BM25) mit Vektorsuche für bessere Genauigkeit (unterstützt von Supabase/Qdrant).
*   **Metadata Filtering**: Filtern Sie Chunks nach Metadaten (z. B. `userId`, `docType`) vor dem Abruf, um Sicherheit und Relevanz zu gewährleisten.
*   **Multi-Query Retriever**: Generiert mehrere Variationen der Benutzerfrage, um bessere Treffer zu finden.

---

## Chains vs. Agenten

| Merkmal | Chain | Agent |
| :--- | :--- | :--- |
| **Logik** | Fest codierte Sequenz | Dynamisches Denken |
| **Vorhersehbarkeit** | Hoch | Geringer (kann loopen oder halluzinieren) |
| **Kosten** | Niedriger | Höher (mehrere LLM-Aufrufe) |
| **Anwendungsfall** | Zusammenfassung, Übersetzung, einfaches RAG | Komplexe Recherche, mehrstufige Aufgaben, Tool-Nutzung |

### Häufige Chains
*   **Basic LLM Chain**: Einfacher Prompt -> Vervollständigung.
*   **Summarization Chain**: Verarbeitet lange Dokumente mit "Map-Reduce" oder "Refine" Strategien.
*   **Structured Output Parser**: Zwingt das LLM, JSON zurückzugeben, das einem bestimmten Schema (Zod) entspricht.
    *   *Tipp*: Verbinden Sie dies mit dem "Output Parser" Eingang einer Chain, um zuverlässige Daten für nachfolgende Nodes zu erhalten.

---

## Profi-Tipps & Fehlerbehebung

### 1. Debugging von Agenten
*   **Ausführliches Logging**: Überprüfen Sie die Browserkonsole oder die n8n-Ausführungsprotokolle, um den "Gedankenprozess" des Agenten zu sehen (Intermediate Steps).
*   **Looping**: Wenn ein Agent stecken bleibt und dasselbe Tool immer wieder aufruft, überprüfen Sie die Tool-Ausgabe. Wenn das Tool einen Fehler zurückgibt, versucht der Agent es möglicherweise unendlich oft erneut. Fügen Sie Fehlerbehandlung im Tool hinzu, um eine beschreibende Nachricht wie "Fehler: ID nicht gefunden" zurückzugeben, anstatt fehlzuschlagen.

### 2. Modellauswahl
*   **GPT-4o / Claude 3.5 Sonnet**: Am besten für komplexe Agenten und Coding-Aufgaben.
*   **GPT-3.5-Turbo / GPT-4o-mini**: Gut für einfache Klassifizierung, Zusammenfassung und Aufgaben mit hohem Volumen.
*   **Lokale Modelle (Ollama)**: Großartig für Datenschutz, haben aber oft Schwierigkeiten mit komplexem Function Calling. Verwenden Sie den **ReAct Agent** für bessere Ergebnisse mit lokalen Modellen.

### 3. Token-Management
*   **Kontextfenster**: Achten Sie auf das Limit des Modells (z. B. 128k Token).
*   **Kürzung**: Verwenden Sie den `Token Splitter`, um sicherzustellen, dass Sie nicht zu viele Daten an das Modell senden.
*   **Kosten**: Agenten können teuer sein. Verwenden Sie `Max Iterations` am Agent-Node, um Endlosschleifen zu verhindern (Standard ist normalerweise 10-15).

### 4. Strukturierte Ausgabe
Wenn Sie möchten, dass der Agent JSON für den nächsten Node in Ihrem Workflow ausgibt:
1.  Fragen Sie nicht einfach im Prompt danach.
2.  Verwenden Sie den **Structured Output Parser**, der mit der Chain/dem Agenten verbunden ist.
3.  Oder verwenden Sie den "JSON Mode" von OpenAI in der Chat Model Node-Konfiguration.
