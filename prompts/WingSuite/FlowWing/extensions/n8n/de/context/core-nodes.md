# Core Nodes & Logik

Dieser Leitfaden behandelt die wesentlichen Nodes zur Steuerung des Datenflusses, der Logik und der Ausführungsreihenfolge in n8n.

## 1. Logik & Verzweigung

### IF Node
Der fundamentale binäre Entscheidungsträger.
*   **Logik**: Wertet eine Bedingung aus (Wahr/Falsch).
*   **Ausgänge**: Hat einen `True` (Wahr) und einen `False` (Falsch) Zweig.
*   **Vergleichstypen**:
    *   **String**: Equals (Gleich), Contains (Enthält), Starts With (Beginnt mit), Regex.
    *   **Number**: >, <, =, >=, <=.
    *   **Boolean**: Is True/False.
    *   **Date**: Is After (Ist nach), Is Before (Ist vor).
    *   **Empty**: Is Empty (Ist leer) / Is Not Empty (Ist nicht leer).
*   **Anwendungsfall**: "Wenn E-Mail '@gmail.com' enthält, leite zur Persönlichen Liste, sonst zur Geschäftsliste."

### Switch Node
Mehrweg-Routing basierend auf Regeln.
*   **Logik**: Wertet einen Eingabewert gegen mehrere Regeln aus.
*   **Ausgänge**: Erstellt einen Ausgang 0, 1, 2... für jede definierte Regel.
*   **Routing**:
    *   **First Match**: Leitet das Item zur *ersten* Regel, die zutrifft.
    *   **All Matches**: Leitet das Item zu *allen* Regeln, die zutreffen (dupliziert das Item).
*   **Fallback**: Kann einen Fallback-Ausgang für Items definieren, die auf keine Regel zutreffen.
*   **Anwendungsfall**: "Support-Tickets basierend auf dem Feld 'Priorität' routen: Hoch -> Slack, Mittel -> E-Mail, Niedrig -> Datenbank."

## 2. Daten zusammenführen (Merging)

Der **Merge Node** ist entscheidend, um geteilte Workflows wieder zusammenzuführen oder Datensätze zu verbinden.

### Modi
1.  **Append**:
    *   **Verhalten**: Stapelt Items von Eingang 2 *nach* Items von Eingang 1.
    *   **Ergebnis**: Eine lange Liste.
    *   **Anwendungsfall**: Kombinieren von zwei Lead-Listen aus verschiedenen Quellen.
2.  **Merge by Key (Join)**:
    *   **Verhalten**: Wie ein SQL JOIN. Gleicht Items von Eingang 1 und Eingang 2 basierend auf einem gemeinsamen Feld (z. B. `email`) ab.
    *   **Typen**:
        *   *Inner Join*: Behalte nur Übereinstimmungen.
        *   *Left Join*: Behalte alle von Eingang 1, füge Daten von Eingang 2 hinzu, falls gefunden.
    *   **Anwendungsfall**: Anreichern einer Liste von Benutzer-IDs mit Benutzerdetails aus einem separaten API-Aufruf.
3.  **Merge by Position**:
    *   **Verhalten**: Führt Item 1 von Eingang 1 mit Item 1 von Eingang 2 zusammen.
    *   **Anwendungsfall**: Wenn Sie wissen, dass die Reihenfolge identisch ist (selten sicher).
4.  **Wait (Pass Through)**:
    *   **Verhalten**: Wartet, bis *beide* Eingänge die Ausführung beendet haben, bevor fortgefahren wird. Leitet Daten von *einem* gewählten Eingang weiter.
    *   **Anwendungsfall**: Sicherstellen, dass zwei parallele Aufgaben (z. B. "Benutzer erstellen" und "Ordner erstellen") beide erledigt sind, bevor eine "Willkommens-E-Mail" gesendet wird.

## 3. Schleifen & Ratenbegrenzung

n8n iteriert bei den meisten Nodes automatisch über Items. Für **Ratenbegrenzung** oder **Sequentielle Verarbeitung** müssen Sie jedoch den **Split In Batches** Node verwenden.

### Das "Split In Batches" Muster
Dies ist das Standardmuster für die Verarbeitung großer Listen oder ratenbegrenzter APIs.

**Struktur:**
1.  **Split In Batches**:
    *   *Batch Size*: `1` (für strikte Ratenlimits) oder `10` (für Bulk-APIs).
2.  **Verarbeitungs-Nodes**:
    *   Verbinden Sie mit dem **Loop** Ausgang von Split In Batches.
    *   Führen Sie Aktionen aus (z. B. HTTP Request).
3.  **Wait Node** (Optional):
    *   Fügen Sie eine Verzögerung hinzu (z. B. `1 Sekunde`), um API-Limits einzuhalten.
4.  **Loop Back**:
    *   Verbinden Sie den Ausgang des letzten Nodes in der Kette *zurück* zum **Eingang** von Split In Batches.

**Wie es funktioniert**:
*   Der Node nimmt den ersten Batch.
*   Verarbeitet ihn.
*   Die Rückverbindung löst den Split In Batches Node aus, um den *nächsten* Batch freizugeben.
*   Wenn keine Items mehr übrig sind, feuert der **Done** Ausgang.

## 4. Datenmanipulation

### Edit Fields (Set)
Der primäre Node für einfache Datentransformation.
*   **Operationen**:
    *   **Set**: Erstellen oder überschreiben eines Feldes.
    *   **Remove**: Löschen eines Feldes.
    *   **Keep Only**: Löschen aller Felder *außer* den angegebenen.
*   **Werte**: Können statische Strings, Zahlen oder Ausdrücke (`{{ $json.field }}`) sein.

### Aggregate
Gruppiert mehrere Items in ein einzelnes Item.
*   **Modi**:
    *   **Aggregate All**: Verwandelt 10 Items in 1 Item, das eine Liste enthält.
    *   **Group By**: Gruppiert Items nach einem bestimmten Feld (z. B. "Kategorie").
*   **Anwendungsfall**: Erstellen einer Zusammenfassungs-E-Mail ("Hier sind die 5 heute erledigten Aufgaben").

### Limit
Reduziert die Anzahl der Items.
*   **Anwendungsfall**: Testen von Workflows (nur die ersten 5 Items verarbeiten).

## 5. Dienstprogramme (Utilities)

### Wait
Pausiert die Ausführung.
*   **Resume On**:
    *   **Time**: Nach X Sekunden/Minuten/Stunden.
    *   **Date**: Zu einem bestimmten Zeitstempel.
    *   **Webhook**: Generiert eine eindeutige "Resume URL". Der Workflow pausiert, bis diese URL aufgerufen wird (z. B. durch ein externes Genehmigungssystem).

### Execute Command
Führt Shell-Befehle auf dem Host-Betriebssystem aus.
*   **Verfügbarkeit**: Nur Self-hosted.
*   **Sicherheit**: Gefährlich. Stellen Sie sicher, dass Eingaben bereinigt sind.
*   **Anwendungsfall**: Ausführen eines Python-Skripts, Verschieben von Dateien auf der Festplatte, Aufrufen von `ffmpeg`.

### No Op (No Operation)
Tut nichts.
*   **Anwendungsfall**: Ein Platzhalter zum Ausrichten des Layouts oder Zusammenführen von Zweigen, wo keine Aktion erforderlich ist.

## 6. Fehlerbehandlung (Core)

### Stop Workflow
Zwingt den Workflow zum Stoppen.
*   **Anwendungsfall**: In einem IF Node, wenn eine kritische Validierung fehlschlägt, leiten Sie zu Stop Workflow.

### Error Trigger
Ein spezieller Trigger Node, der *nur* läuft, wenn ein anderer Workflow fehlschlägt.
*   **Anwendungsfall**: Erstellen eines globalen "Error Handler" Workflows, der Alarme an Slack/E-Mail sendet, wann immer ein Produktions-Workflow abstürzt.
