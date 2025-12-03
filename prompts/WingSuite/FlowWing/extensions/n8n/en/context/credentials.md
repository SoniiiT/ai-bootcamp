---
description: Comprehensive guide to managing authentication and credentials in n8n.
globs: **/*.json
---

# Credentials & Authentication Guide

This guide explains how n8n handles authentication, from simple API keys to complex OAuth2 flows, and how to manage credentials securely.

## Core Concepts

### Separation of Concerns
n8n decouples authentication from workflow logic.
*   **Credentials**: Stored securely in the database (encrypted).
*   **Nodes**: Reference credentials by ID.
*   **Benefit**: You can update an API key in one place, and all 50 workflows using it will update automatically.

### Encryption
All credentials are encrypted at rest using the `N8N_ENCRYPTION_KEY`.
*   **Critical**: If you lose this key (stored in `.n8n/config` or env var), **ALL** credentials become unreadable and must be recreated.
*   **Backup**: Always backup your encryption key when backing up the database.

---

## Credential Types

### 1. Predefined Credentials (Recommended)
n8n has hundreds of built-in credential types for specific services (e.g., "Google Sheets OAuth2 API", "Slack API").
*   **Why use them?**: They handle the specific quirks of the API (token refresh, header placement, specialized scopes).
*   **Setup**: Select the service in the node, then create the credential.

### 2. Generic Credentials (HTTP Request)
When a specific service isn't supported, use generic auth types with the **HTTP Request** node.

| Type | Use Case | Configuration |
| :--- | :--- | :--- |
| **Header Auth** | API Key in Header | Name: `X-API-Key`, Value: `abc...` |
| **Query Auth** | API Key in URL | Name: `api_token`, Value: `abc...` |
| **Basic Auth** | Username/Password | User: `admin`, Pass: `secret` |
| **Bearer Auth** | Bearer Token | Token: `eyJ...` (Auto-adds `Authorization: Bearer ...`) |
| **Digest Auth** | Legacy/IoT | User, Password, Realm |
| **OAuth2 API** | Complex Auth | Client ID, Secret, Auth URL, Token URL |

---

## OAuth2 Deep Dive

OAuth2 is the most complex auth type. n8n handles the "dance" (redirect, code exchange, token refresh) for you.

### Grant Types
1.  **Authorization Code**:
    *   **User Context**: Requires a user to log in via a browser.
    *   **Setup**: You need a "Client ID" and "Client Secret" from the provider (e.g., Google Cloud Console).
    *   **Redirect URL**: You must register n8n's callback URL with the provider.
        *   Cloud: `https://<your-instance>.app.n8n.cloud/rest/oauth2-credential/callback`
        *   Self-hosted: `https://<your-domain>/rest/oauth2-credential/callback`
2.  **Client Credentials**:
    *   **Machine Context**: No user login. Server-to-Server.
    *   **Setup**: Client ID and Client Secret. No Redirect URL needed usually.

### Scopes
*   **Definition**: Permissions you are requesting (e.g., `https://www.googleapis.com/auth/spreadsheets.readonly`).
*   **Tip**: If an API call fails with 403 Forbidden, check if your credential has the correct Scope. You may need to re-authenticate to grant new scopes.

### Token Refresh
n8n automatically refreshes Access Tokens using the Refresh Token.
*   **Troubleshooting**: If a workflow fails with "Token expired" or "Unauthorized", it means the Refresh Token itself has expired or was revoked. You must manually "Reconnect" the credential in the UI.

---

## Managing Credentials

### Sharing
*   **Personal**: By default, credentials belong to the creator.
*   **Shared**: You can share credentials with other users (if on a Team/Enterprise plan).
    *   *Permissions*: "Use" (can run workflows) vs "Manage" (can edit the secret).

### Environment Variables
For static secrets (like database passwords or internal API keys), you can avoid the Credential UI and use Environment Variables.
*   **Usage**: In a node parameter (or Generic Credential value), use an Expression: `{{ $env.MY_SECRET_KEY }}`.
*   **Benefit**: easier to manage in CI/CD or Infrastructure-as-Code setups.

### External Secrets (Enterprise)
Connect n8n to AWS Secrets Manager, HashiCorp Vault, etc.
*   **Usage**: `{{ $secrets.vault.myKey }}`.
*   **See**: `administration.md` for setup details.

---

## Scripting with Credentials

### Code Node Access
For security, credentials are **NOT** automatically available in the Code node. You must request them.

1.  **Request Access**: In the Code node, go to "Input" -> "Credential" and select the credential you need.
2.  **Access in Code**:
    ```javascript
    // The credential object is available via this helper
    const creds = await this.getCredentials('myCredentialName');
    
    // Access properties
    const apiKey = creds.apiKey;
    const password = creds.password;
    ```

### HTTP Request Node
The HTTP Request node handles auth automatically. You rarely need to manually access the credential value in an expression unless you are building a custom auth header manually.

---

## Troubleshooting

### "Redirect URL Mismatch" (OAuth2)
*   **Cause**: The URL in your n8n instance (`WEBHOOK_URL`) does not match what you registered in the Google/Microsoft/Slack console.
*   **Fix**: Ensure `WEBHOOK_URL` is set correctly (https://...) and matches the provider's allowlist exactly.

### "Credential not found"
*   **Cause**: You imported a workflow, but the credential ID referenced in the JSON doesn't exist in your instance.
*   **Fix**: Create a new credential and link it in the node.

### "Decryption failed"
*   **Cause**: The `N8N_ENCRYPTION_KEY` changed or the database was moved to a new instance without the key.
*   **Fix**: Restore the original key. There is no way to recover credentials without it.
