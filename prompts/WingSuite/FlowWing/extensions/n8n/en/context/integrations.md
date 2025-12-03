---
description: Comprehensive guide to using built-in integrations, community nodes, and custom API actions in n8n.
globs: **/*.json
---

# Integrations & Nodes Guide

n8n connects to hundreds of services. This guide explains how to choose the right integration method and handle common patterns like polling, webhooks, and rate limiting.

## Integration Hierarchy

When connecting to a service, follow this hierarchy of preference:

1.  **Built-in Node**: (e.g., Google Sheets, Slack). Best for standard tasks. Handles auth, pagination, and error parsing.
2.  **Built-in Node (Custom API Call)**: Best for accessing endpoints not yet exposed in the node's UI, while still using the node's managed authentication.
3.  **Community Node**: Best for niche services not supported natively.
4.  **HTTP Request Node**: The fallback. Best for raw control or unsupported services.

---

## 1. Built-in Nodes

### The "Custom API Call" Feature
Many built-in nodes have a hidden superpower: the **Custom API Call** resource (sometimes called "Other" or "Raw").

*   **Why use it?**: You need to call an endpoint (e.g., `/users/ban`) that isn't a button in the UI, but you don't want to manually configure OAuth2 in an HTTP Request node.
*   **How to use**:
    1.  Open the node (e.g., Slack).
    2.  Set **Resource** to `Other` or `Custom API Call`.
    3.  Set **Operation** to the HTTP Method (GET, POST, etc.).
    4.  Enter the **URL** (usually relative, e.g., `/chat.postMessage`).
    5.  n8n automatically injects the authentication headers.

### Common Node Specifics

#### Google Sheets
*   **Key Row**: Always ensure your sheet has a header row. n8n uses this to map JSON keys.
*   **ValueRenderOption**: Set to `UNFORMATTED_VALUE` if you want numbers/dates as raw data, or `FORMATTED_VALUE` for strings.
*   **Upsert**: To update a row if it exists or create it if not, use the "Update" operation and logic in your workflow to check for existence first (or use the "Append or Update" operation if available).

#### Slack / Microsoft Teams
*   **Rich Messages**: Use "Block Kit" (Slack) or "Adaptive Cards" (Teams).
*   **Builder**: Use the official Block Kit Builder website to design your message, then copy the JSON into the node's "Blocks" parameter (using an Expression).

#### SQL Nodes (Postgres, MySQL)
*   **Execute Query**: The most flexible operation. You can write raw SQL.
*   **Parameters**: Use `$1`, `$2` (Postgres) or `?` (MySQL) for parameterized queries to prevent SQL injection.
    *   *Example*: `SELECT * FROM users WHERE email = $1`
    *   *Query Parameters*: `{{ $json.email }}`

---

## 2. Triggers

### Webhook Triggers (Real-time)
*   **Mechanism**: The service pushes data to n8n.
*   **Requirement**: n8n must be accessible from the internet (Public IP or Tunnel).
*   **Security**: Always configure "Authentication" (Basic Auth, Header Auth) or validate the signature (HMAC) in the webhook node settings.

### Polling Triggers (Scheduled)
*   **Mechanism**: n8n asks "Is there anything new?" every X minutes.
*   **Deduplication**: n8n stores the ID of the last processed item in its database ("Static Data").
*   **Reset**: To re-process old items, you must clear the static data (Workflow Settings > "Clear static data").
*   **Limitations**: Not real-time. Minimum interval is usually 1 minute.

---

## 3. Community Nodes

Community nodes are plugins created by the n8n ecosystem.

*   **Installation**: **Settings > Community Nodes > Install**. Enter the npm package name (e.g., `n8n-nodes-browserless`).
*   **Security**: These run in the same process as n8n. Only install trusted nodes.
*   **Updates**: You must manually check for and install updates via the Settings menu.

---

## 4. Integration Patterns

### Rate Limiting (Throttling)
APIs often limit requests (e.g., 60 per minute).

1.  **Split In Batches**: Process items in chunks (e.g., 10 items).
2.  **Wait Node**: Add a delay after each batch.
    *   *Example*: Batch Size 1, Wait 1s = 60 requests/minute.
3.  **Retry on 429**: Configure the HTTP Request node to "Retry on Fail" and specifically handle status code `429`.

### Pagination
*   **Built-in Nodes**: Usually have a "Return All" toggle. Enable it, and n8n handles the looping.
*   **HTTP Request**: You must build a loop.
    1.  **Fetch**: Get page 1.
    2.  **Check**: Is there a `next_page` token?
    3.  **Loop**: If yes, pass the token to the next request.
    *   *See `http-request.md` for a detailed pagination recipe.*

### Upsert (Update or Insert)
Synchronizing data between two systems (e.g., CRM -> Database).

1.  **Try to Find**: Search for the record by a unique key (Email, ID).
2.  **IF Node**:
    *   **True (Found)**: Update the existing record.
    *   **False (Not Found)**: Create a new record.
3.  **Merge**: Use a Merge node to bring the branches back together if needed.

---

## 5. Handling Files (Binary Data)
Most integrations handle files via the **Binary Property**.

*   **Upload**: Ensure the input item has the file in a binary property (default `data`). Enable "Send Binary Data" in the node.
*   **Download**: The node will output a binary property. You may need to use **Move Binary Data** to rename it or convert it to base64 if the next API expects a base64 string in the JSON body.
