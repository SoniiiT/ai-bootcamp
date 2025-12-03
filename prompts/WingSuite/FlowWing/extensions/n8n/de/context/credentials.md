---
description: Umfassender Leitfaden zur Verwaltung von Authentifizierung und Zugangsdaten in n8n.
globs: **/*.json
---

# Handbuch für Zugangsdaten & Authentifizierung

Dieser Leitfaden erklärt, wie n8n die Authentifizierung handhabt, von einfachen API-Schlüsseln bis hin zu komplexen OAuth2-Flows, und wie man Zugangsdaten sicher verwaltet.

## Kernkonzepte

### Trennung der Zuständigkeiten (Separation of Concerns)
n8n entkoppelt die Authentifizierung von der Workflow-Logik.
*   **Credentials**: Werden sicher in der Datenbank gespeichert (verschlüsselt).
*   **Nodes**: Referenzieren Credentials über eine ID.
*   **Vorteil**: Sie können einen API-Schlüssel an einer Stelle aktualisieren, und alle 50 Workflows, die ihn verwenden, werden automatisch aktualisiert.

### Verschlüsselung
Alle Zugangsdaten werden im Ruhezustand mit dem `N8N_ENCRYPTION_KEY` verschlüsselt.
*   **Kritisch**: Wenn Sie diesen Schlüssel verlieren (gespeichert in `.n8n/config` oder Env-Var), werden **ALLE** Zugangsdaten unlesbar und müssen neu erstellt werden.
*   **Backup**: Sichern Sie immer Ihren Verschlüsselungsschlüssel, wenn Sie die Datenbank sichern.

---

## Credential-Typen

### 1. Vordefinierte Credentials (Empfohlen)
n8n verfügt über Hunderte von eingebauten Credential-Typen für spezifische Dienste (z. B. "Google Sheets OAuth2 API", "Slack API").
*   **Warum diese verwenden?**: Sie behandeln die spezifischen Eigenheiten der API (Token-Refresh, Header-Platzierung, spezialisierte Scopes).
*   **Einrichtung**: Wählen Sie den Dienst im Node aus und erstellen Sie dann das Credential.

### 2. Generische Credentials (HTTP Request)
Wenn ein spezifischer Dienst nicht unterstützt wird, verwenden Sie generische Auth-Typen mit dem **HTTP Request** Node.

| Typ | Anwendungsfall | Konfiguration |
| :--- | :--- | :--- |
| **Header Auth** | API-Schlüssel im Header | Name: `X-API-Key`, Wert: `abc...` |
| **Query Auth** | API-Schlüssel in URL | Name: `api_token`, Wert: `abc...` |
| **Basic Auth** | Benutzername/Passwort | User: `admin`, Pass: `geheim` |
| **Bearer Auth** | Bearer Token | Token: `eyJ...` (Fügt automatisch `Authorization: Bearer ...` hinzu) |
| **Digest Auth** | Legacy/IoT | User, Password, Realm |
| **OAuth2 API** | Komplexe Auth | Client ID, Secret, Auth URL, Token URL |

---

## OAuth2 Deep Dive

OAuth2 ist der komplexeste Auth-Typ. n8n übernimmt den "Tanz" (Redirect, Code-Austausch, Token-Refresh) für Sie.

### Grant Types
1.  **Authorization Code**:
    *   **Benutzerkontext**: Erfordert, dass sich ein Benutzer über einen Browser anmeldet.
    *   **Einrichtung**: Sie benötigen eine "Client ID" und ein "Client Secret" vom Anbieter (z. B. Google Cloud Console).
    *   **Redirect URL**: Sie müssen die Callback-URL von n8n beim Anbieter registrieren.
        *   Cloud: `https://<ihre-instanz>.app.n8n.cloud/rest/oauth2-credential/callback`
        *   Self-hosted: `https://<ihre-domain>/rest/oauth2-credential/callback`
2.  **Client Credentials**:
    *   **Maschinenkontext**: Kein Benutzer-Login. Server-zu-Server.
    *   **Einrichtung**: Client ID und Client Secret. Normalerweise keine Redirect URL erforderlich.

### Scopes
*   **Definition**: Berechtigungen, die Sie anfordern (z. B. `https://www.googleapis.com/auth/spreadsheets.readonly`).
*   **Tipp**: Wenn ein API-Aufruf mit 403 Forbidden fehlschlägt, prüfen Sie, ob Ihr Credential den korrekten Scope hat. Möglicherweise müssen Sie sich neu authentifizieren, um neue Scopes zu gewähren.

### Token Refresh
n8n aktualisiert Access Tokens automatisch unter Verwendung des Refresh Tokens.
*   **Fehlerbehebung**: Wenn ein Workflow mit "Token expired" oder "Unauthorized" fehlschlägt, bedeutet dies, dass das Refresh Token selbst abgelaufen ist oder widerrufen wurde. Sie müssen das Credential in der UI manuell "neu verbinden" (Reconnect).

---

## Verwaltung von Credentials

### Teilen (Sharing)
*   **Persönlich**: Standardmäßig gehören Credentials dem Ersteller.
*   **Geteilt**: Sie können Credentials mit anderen Benutzern teilen (wenn Sie einen Team-/Enterprise-Plan haben).
    *   *Berechtigungen*: "Use" (kann Workflows ausführen) vs. "Manage" (kann das Geheimnis bearbeiten).

### Umgebungsvariablen
Für statische Geheimnisse (wie Datenbankpasswörter oder interne API-Schlüssel) können Sie die Credential-UI vermeiden und Umgebungsvariablen verwenden.
*   **Verwendung**: In einem Node-Parameter (oder generischen Credential-Wert) verwenden Sie einen Ausdruck: `{{ $env.MY_SECRET_KEY }}`.
*   **Vorteil**: Einfacher zu verwalten in CI/CD- oder Infrastructure-as-Code-Setups.

### Externe Geheimnisse (Enterprise)
Verbinden Sie n8n mit AWS Secrets Manager, HashiCorp Vault, usw.
*   **Verwendung**: `{{ $secrets.vault.myKey }}`.
*   **Siehe**: `administration.md` für Einrichtungsdetails.

---

## Scripting mit Credentials

### Code Node Zugriff
Aus Sicherheitsgründen sind Credentials **NICHT** automatisch im Code Node verfügbar. Sie müssen sie anfordern.

1.  **Zugriff anfordern**: Gehen Sie im Code Node zu "Input" -> "Credential" und wählen Sie das benötigte Credential aus.
2.  **Zugriff im Code**:
    ```javascript
    // Das Credential-Objekt ist über diesen Helfer verfügbar
    const creds = await this.getCredentials('myCredentialName');
    
    // Zugriff auf Eigenschaften
    const apiKey = creds.apiKey;
    const password = creds.password;
    ```

### HTTP Request Node
Der HTTP Request Node handhabt die Authentifizierung automatisch. Sie müssen selten manuell auf den Credential-Wert in einem Ausdruck zugreifen, es sei denn, Sie bauen manuell einen benutzerdefinierten Auth-Header.

---

## Fehlerbehebung

### "Redirect URL Mismatch" (OAuth2)
*   **Ursache**: Die URL in Ihrer n8n-Instanz (`WEBHOOK_URL`) stimmt nicht mit dem überein, was Sie in der Google/Microsoft/Slack-Konsole registriert haben.
*   **Lösung**: Stellen Sie sicher, dass `WEBHOOK_URL` korrekt gesetzt ist (https://...) und exakt mit der Allowlist des Anbieters übereinstimmt.

### "Credential not found"
*   **Ursache**: Sie haben einen Workflow importiert, aber die im JSON referenzierte Credential-ID existiert in Ihrer Instanz nicht.
*   **Lösung**: Erstellen Sie ein neues Credential und verknüpfen Sie es im Node.

### "Decryption failed"
*   **Ursache**: Der `N8N_ENCRYPTION_KEY` wurde geändert oder die Datenbank wurde ohne den Schlüssel auf eine neue Instanz verschoben.
*   **Lösung**: Stellen Sie den ursprünglichen Schlüssel wieder her. Es gibt keine Möglichkeit, Credentials ohne ihn wiederherzustellen.
