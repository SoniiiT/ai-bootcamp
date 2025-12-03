---
description: Umfassender Leitfaden zur Verwendung von integrierten Integrationen, Community Nodes und benutzerdefinierten API-Aktionen in n8n.
globs: **/*.json
---

# Integrations & Nodes Handbuch

n8n verbindet sich mit Hunderten von Diensten. Dieser Leitfaden erklärt, wie Sie die richtige Integrationsmethode wählen und gängige Muster wie Polling, Webhooks und Ratenbegrenzung handhaben.

## Integrations-Hierarchie

Wenn Sie sich mit einem Dienst verbinden, folgen Sie dieser Präferenzhierarchie:

1.  **Built-in Node**: (z. B. Google Sheets, Slack). Am besten für Standardaufgaben. Handhabt Authentifizierung, Paginierung und Fehler-Parsing.
2.  **Built-in Node (Custom API Call)**: Am besten für den Zugriff auf Endpunkte, die noch nicht in der Benutzeroberfläche des Nodes verfügbar sind, während die verwaltete Authentifizierung des Nodes weiterhin genutzt wird.
3.  **Community Node**: Am besten für Nischendienste, die nicht nativ unterstützt werden.
4.  **HTTP Request Node**: Der Fallback. Am besten für rohe Kontrolle oder nicht unterstützte Dienste.

---

## 1. Built-in Nodes

### Die "Custom API Call" Funktion
Viele eingebaute Nodes haben eine versteckte Superkraft: die **Custom API Call** Ressource (manchmal "Other" oder "Raw" genannt).

*   **Warum nutzen?**: Sie müssen einen Endpunkt aufrufen (z. B. `/users/ban`), der kein Button in der UI ist, aber Sie möchten OAuth2 nicht manuell in einem HTTP Request Node konfigurieren.
*   **Wie man es nutzt**:
    1.  Öffnen Sie den Node (z. B. Slack).
    2.  Setzen Sie **Resource** auf `Other` oder `Custom API Call`.
    3.  Setzen Sie **Operation** auf die HTTP-Methode (GET, POST, etc.).
    4.  Geben Sie die **URL** ein (normalerweise relativ, z. B. `/chat.postMessage`).
    5.  n8n fügt automatisch die Authentifizierungs-Header ein.

### Gängige Node-Spezifika

#### Google Sheets
*   **Key Row**: Stellen Sie immer sicher, dass Ihr Blatt eine Kopfzeile hat. n8n verwendet diese, um JSON-Schlüssel zuzuordnen.
*   **ValueRenderOption**: Setzen Sie dies auf `UNFORMATTED_VALUE`, wenn Sie Zahlen/Daten als Rohdaten wünschen, oder `FORMATTED_VALUE` für Strings.
*   **Upsert**: Um eine Zeile zu aktualisieren, wenn sie existiert, oder sie zu erstellen, wenn nicht, verwenden Sie die "Update"-Operation und Logik in Ihrem Workflow, um zuerst auf Existenz zu prüfen (oder verwenden Sie die "Append or Update"-Operation, falls verfügbar).

#### Slack / Microsoft Teams
*   **Rich Messages**: Verwenden Sie "Block Kit" (Slack) oder "Adaptive Cards" (Teams).
*   **Builder**: Verwenden Sie die offizielle Block Kit Builder-Website, um Ihre Nachricht zu entwerfen, und kopieren Sie dann das JSON in den "Blocks"-Parameter des Nodes (unter Verwendung eines Ausdrucks).

#### SQL Nodes (Postgres, MySQL)
*   **Execute Query**: Die flexibelste Operation. Sie können rohes SQL schreiben.
*   **Parameters**: Verwenden Sie `$1`, `$2` (Postgres) oder `?` (MySQL) für parametrisierte Abfragen, um SQL-Injection zu verhindern.
    *   *Beispiel*: `SELECT * FROM users WHERE email = $1`
    *   *Query Parameters*: `{{ $json.email }}`

---

## 2. Trigger

### Webhook Trigger (Echtzeit)
*   **Mechanismus**: Der Dienst pusht Daten an n8n.
*   **Voraussetzung**: n8n muss aus dem Internet erreichbar sein (Öffentliche IP oder Tunnel).
*   **Sicherheit**: Konfigurieren Sie immer "Authentication" (Basic Auth, Header Auth) oder validieren Sie die Signatur (HMAC) in den Webhook-Node-Einstellungen.

### Polling Trigger (Geplant)
*   **Mechanismus**: n8n fragt alle X Minuten "Gibt es etwas Neues?".
*   **Deduplizierung**: n8n speichert die ID des zuletzt verarbeiteten Elements in seiner Datenbank ("Static Data").
*   **Reset**: Um alte Elemente erneut zu verarbeiten, müssen Sie die statischen Daten löschen (Workflow Settings > "Clear static data").
*   **Einschränkungen**: Nicht in Echtzeit. Das Mindestintervall beträgt normalerweise 1 Minute.

---

## 3. Community Nodes

Community Nodes sind Plugins, die vom n8n-Ökosystem erstellt wurden.

*   **Installation**: **Settings > Community Nodes > Install**. Geben Sie den npm-Paketnamen ein (z. B. `n8n-nodes-browserless`).
*   **Sicherheit**: Diese laufen im selben Prozess wie n8n. Installieren Sie nur vertrauenswürdige Nodes.
*   **Updates**: Sie müssen manuell über das Einstellungsmenü nach Updates suchen und diese installieren.

---

## 4. Integrations-Muster

### Ratenbegrenzung (Throttling)
APIs begrenzen oft Anfragen (z. B. 60 pro Minute).

1.  **Split In Batches**: Verarbeiten Sie Elemente in Blöcken (z. B. 10 Elemente).
2.  **Wait Node**: Fügen Sie nach jedem Block eine Verzögerung hinzu.
    *   *Beispiel*: Batch-Größe 1, Wartezeit 1s = 60 Anfragen/Minute.
3.  **Retry on 429**: Konfigurieren Sie den HTTP Request Node auf "Retry on Fail" und behandeln Sie speziell den Statuscode `429`.

### Paginierung
*   **Built-in Nodes**: Haben normalerweise einen "Return All"-Schalter. Aktivieren Sie ihn, und n8n übernimmt das Looping.
*   **HTTP Request**: Sie müssen eine Schleife bauen.
    1.  **Fetch**: Seite 1 abrufen.
    2.  **Check**: Gibt es ein `next_page`-Token?
    3.  **Loop**: Wenn ja, übergeben Sie das Token an die nächste Anfrage.
    *   *Siehe `http-request.md` für ein detailliertes Paginierungs-Rezept.*

### Upsert (Update oder Insert)
Synchronisieren von Daten zwischen zwei Systemen (z. B. CRM -> Datenbank).

1.  **Try to Find**: Suchen Sie nach dem Datensatz anhand eines eindeutigen Schlüssels (E-Mail, ID).
2.  **IF Node**:
    *   **True (Gefunden)**: Aktualisieren Sie den vorhandenen Datensatz.
    *   **False (Nicht gefunden)**: Erstellen Sie einen neuen Datensatz.
3.  **Merge**: Verwenden Sie einen Merge Node, um die Zweige bei Bedarf wieder zusammenzuführen.

---

## 5. Umgang mit Dateien (Binärdaten)
Die meisten Integrationen behandeln Dateien über die **Binary Property**.

*   **Upload**: Stellen Sie sicher, dass das Eingabeelement die Datei in einer Binäreigenschaft hat (Standard `data`). Aktivieren Sie "Send Binary Data" im Node.
*   **Download**: Der Node gibt eine Binäreigenschaft aus. Möglicherweise müssen Sie **Move Binary Data** verwenden, um sie umzubenennen oder in Base64 zu konvertieren, wenn die nächste API einen Base64-String im JSON-Body erwartet.
