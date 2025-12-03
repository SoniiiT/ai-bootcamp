---
description: Leitfaden zur Erweiterung von n8n mit benutzerdefinierten Nodes, API-Aktionen und LangChain-Code
---

# Benutzerdefinierte Erweiterungen & Fortgeschrittener Code

Wenn eingebaute Nodes nicht ausreichen, bietet n8n Mechanismen zur Erweiterung der Funktionalität durch benutzerdefinierte API-Aufrufe, spezialisierte Code Nodes und die Entwicklung eigener Nodes.

## 1. Benutzerdefinierte API-Aktionen (Der "Fehlender Node" Fix)

Wenn ein Node für einen Dienst existiert (z. B. Slack), aber eine bestimmte Operation fehlt (z. B. "Admin Archive Channel"), oder wenn kein Node existiert, der Dienst aber eine API hat, verwenden Sie den **HTTP Request** Node.

### Verwendung vordefinierter Credentials
Sie können Credentials von bestehenden Nodes in einem generischen HTTP Request verwenden. Dies ist die **#1 Best Practice** zur Erweiterung von n8n.

1.  **HTTP Request Node hinzufügen**.
2.  **Authentifizierung**: Wählen Sie **Predefined Credential Type**.
3.  **Credential Type**: Suchen Sie nach dem Dienst (z. B. "Google Calendar OAuth2 API").
    *   *Hinweis*: Auch wenn der Node "Google Calendar" heißt, kann das Credential "Google Calendar OAuth2 API" heißen.
4.  **Credential**: Wählen Sie Ihr bestehendes authentifiziertes Konto.
5.  **URL**: Geben Sie den spezifischen API-Endpunkt aus der Dokumentation des Dienstes ein.

**Vorteil**: n8n übernimmt den OAuth2 Token-Refresh, die Header-Injektion und die Sicherheit. Sie geben nur die URL und den Body an.

### Credential-Only Nodes
Einige Integrationen sind "Credential-only". Sie erscheinen im Nodes-Panel, bieten aber nur Credentials zur Verwendung mit dem HTTP Request Node.
*   **Beispiele**: Carbon Black, bestimmte AWS-Dienste.
*   **Verwendung**: Node installieren -> Credential erstellen -> Im HTTP Request verwenden.

## 2. LangChain Code Node

Der **LangChain Code Node** ist eine spezialisierte Version des Code Nodes, die speziell für das KI-Agenten-Ökosystem entwickelt wurde. Er ermöglicht es Ihnen, LangChain-Funktionalität zu importieren, die n8n noch nicht gewrappt hat.

### Hauptunterschiede zum Standard Code Node
*   **Sprache**: Unterstützt **nur JavaScript** (kein Python).
*   **Kontext**: Entwickelt, um innerhalb einer KI-Chain oder eines Agenten zu arbeiten.
*   **Eingänge/Ausgänge**: Kann mehrere spezialisierte Eingänge haben (z. B. `ai_memory`, `ai_tool`).

### Modi
1.  **Execute**:
    *   **Verhalten**: Verhält sich wie ein Standard Code Node. Nimmt Eingabe, verarbeitet sie, gibt Ausgabe zurück.
    *   **Anforderung**: Muss einen **Main Input** und **Main Output** haben.
    *   **Anwendungsfall**: Benutzerdefinierte Logik innerhalb einer Chain (z. B. Formatierung eines Prompts vor dem Senden an das LLM).
2.  **Supply Data**:
    *   **Verhalten**: Agiert als Sub-Node (Tool) für einen Root-Node (Agent/Chain).
    *   **Anforderung**: Verwendet **Nicht-Main-Ausgänge** (z. B. Verbindung zum `Tool`-Eingang eines Agenten).
    *   **Anwendungsfall**: Erstellen eines benutzerdefinierten Tools mit JavaScript, das der Agent aufrufen kann.

### Eingebaute Methoden
*   `this.addInputData(inputName, data)`: Mock-Daten für Nicht-Main-Eingänge.
*   `this.getInputData(inputIndex?, inputName?)`: Daten vom Main-Eingang abrufen.
*   `this.getExecutionCancelSignal()`: Abbruch in benutzerdefinierten Chains behandeln.
*   `this.loadModule(moduleName)`: (Nur Self-hosted) Externe LangChain-Module laden, falls aktiviert.

## 3. Erstellen benutzerdefinierter Nodes

Wenn Sie wiederverwendbare Logik über viele Workflows hinweg benötigen oder mit der Community teilen möchten, bauen Sie einen Custom Node.

### Node-Stile

#### A. Deklarativer Stil (JSON)
*   **Am besten für**: Einfache API-Wrapper (REST/HTTP).
*   **Sprache**: JSON.
*   **Struktur**:
    *   `properties`: Definieren Sie UI-Felder (API Key, Ressource, Operation).
    *   `routing`: Definieren Sie, wie Felder auf API-Anfragen abgebildet werden (Methode, URL, Body).
*   **Vorteile**: Einfach zu bauen, keine TypeScript-Kenntnisse erforderlich, n8n übernimmt UI/Ausführung.
*   **Nachteile**: Begrenzte Logik. Kann keine komplexen Datentransformationen oder Nicht-HTTP-Protokolle handhaben.

#### B. Programmatischer Stil (TypeScript)
*   **Am besten für**: Komplexe Logik, Datentransformation oder Nicht-HTTP-Protokolle (TCP, SSH).
*   **Sprache**: TypeScript.
*   **Struktur**:
    *   `description`: UI-Definition.
    *   `execute()`: Die Hauptfunktion. Sie haben die volle Kontrolle.
        *   Eingaben lesen: `this.getNodeParameter('myField')`.
        *   Daten verarbeiten.
        *   JSON zurückgeben.
*   **Vorteile**: Unbegrenzte Möglichkeiten. Kann npm-Pakete verwenden.
*   **Nachteile**: Erfordert Programmierkenntnisse und Build-Schritt.

### Entwicklungs-Workflow
1.  **Scaffold**: Verwenden Sie die `n8n-node-dev` CLI.
    *   `npx n8n-node-dev new`
2.  **Build**: `npm run build`
3.  **Link**: `npm link` zu Ihrer lokalen n8n-Instanz zum Testen.
4.  **Publish**: `npm publish` zur npm-Registry.

### Community Nodes
Benutzerdefinierte Nodes, die auf npm veröffentlicht wurden, können in jeder n8n-Instanz installiert werden.
*   **Installation**: Settings > Community Nodes > Install.
*   **Benennung**: Muss mit `n8n-nodes-` beginnen.

## 4. Externe Geheimnisse (Vaults)

Für Unternehmenssicherheit speichern Sie Credentials nicht in der n8n-Datenbank. Verwenden Sie einen externen Secret Store.

### Unterstützte Anbieter
*   AWS Secrets Manager
*   Azure Key Vault
*   Google Cloud Secret Manager
*   HashiCorp Vault
*   Infisical

### Verwendung
1.  **Verbinden**: Settings > External Secrets.
2.  **Referenzieren**: In jedem Credential-Feld (z. B. "API Key") wechseln Sie den Modus auf **Expression**.
3.  **Syntax**: `{{ $secrets.vaultName.secretName }}`.
    *   *Beispiel*: `{{ $secrets.awsSecretsManager.stripe_api_key }}`.

Dies stellt sicher, dass bei einem Leck Ihres n8n-Backups die tatsächlichen Schlüssel nicht offengelegt werden.
