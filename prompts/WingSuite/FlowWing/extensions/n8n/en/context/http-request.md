# HTTP Request Node Guide

## Overview
The HTTP Request node is the powerhouse of n8n, enabling interaction with any REST, GraphQL, or SOAP API. It serves two primary functions:
1.  **Workflow Step**: Fetching or sending data as part of a linear process.
2.  **AI Tool**: Acting as a function that an AI Agent can call dynamically.

## 1. Configuration & Authentication

### Authentication Methods
Security is handled separately from the node logic via Credentials.

| Method | Use Case | Configuration |
| :--- | :--- | :--- |
| **Predefined** | **Preferred**. Use when n8n has a built-in credential type (e.g., Slack, Google). | Select "Predefined Credential Type" -> Choose Service -> Select Credential. Handles token refresh automatically. |
| **Header Auth** | APIs requiring a static API Key in headers. | Name: `Authorization` (or `X-API-Key`), Value: `Bearer <Key>`. |
| **Query Auth** | APIs requiring the key in the URL. | Name: `api_key`, Value: `<Key>`. |
| **Basic Auth** | Legacy APIs. | Username/Password encoded in Base64. |
| **OAuth2** | Complex flows. | Requires Client ID, Secret, Auth URL, Token URL, and Scopes. n8n handles the callback URL (`.../rest/oauth2-credential/callback`). |
| **Digest Auth** | Higher security than Basic. | Handled automatically by n8n's Digest Auth credential. |

### Request Configuration
*   **Method**: GET, POST, PUT, DELETE, PATCH, HEAD.
*   **URL**: Supports expressions (e.g., `https://api.example.com/users/{{ $json.userId }}`).
*   **Send Query Parameters**:
    *   **Specify Below**: Manually add Key/Value pairs.
    *   **JSON**: Pass a JSON object directly (useful when parameters are dynamic).
    *   **Array Format**: Critical for PHP/Rails APIs.
        *   `No brackets`: `ids=1&ids=2`
        *   `Brackets []`: `ids[]=1&ids[]=2`
        *   `Indices [0]`: `ids[0]=1&ids[1]=2`

### Sending Data (Body)
*   **JSON**: The standard. Automatically adds `Content-Type: application/json`.
*   **Form-Data**: Used for file uploads.
    *   *Parameter Type*: "File".
    *   *Input Data Field Name*: The name of the binary property in n8n (default: `data`).
*   **Form URLencoded**: Used by older APIs (e.g., Twilio). `application/x-www-form-urlencoded`.
*   **Raw**: For XML, SOAP, or plain text. You must set the `Content-Type` header manually.

## 2. Response Handling & Parsing

### Response Format
*   **Autodetect**: n8n tries to parse JSON. If it fails, it returns text.
*   **JSON**: Forces JSON parsing. Errors if the response is HTML/Text.
*   **Binary**: **Crucial for files**.
    *   *Response Data Property Name*: Where to store the file (e.g., `data`).
    *   Use this when downloading PDFs, Images, or CSVs.
*   **Text**: Returns the raw body string.

### Error Handling Options
*   **Never Error**:
    *   **Enabled**: The node *always* succeeds (green output).
    *   **Output**: Adds an `error` object to the output JSON if the status is 4xx/5xx.
    *   **Use Case**: When you want to handle 404s (Not Found) logically in an IF node (e.g., "If 404, create user; else update user").
*   **Include Response Headers**:
    *   **Enabled**: Returns headers like `x-rate-limit-remaining`.
    *   **Use Case**: Building robust rate-limiting logic.

## 3. Pagination (Looping)

Automates fetching large datasets (e.g., "Get all 5000 users").

### Mode 1: Response Contains Next URL
The API returns a full URL for the next page (e.g., `next: "https://api.com?page=2"`).
*   **Next URL**: Expression pointing to the field, e.g., `{{ $response.body.links.next }}`.
*   **Stop Condition**: Automatically stops when the field is missing or null.

### Mode 2: Update a Parameter
The API uses `page` or `offset` parameters.
*   **Type**: "Offset" (0, 10, 20) or "Page Number" (1, 2, 3).
*   **Parameter Name**: The query param to update (e.g., `page`).
*   **Increment**: How much to add per loop (usually `1` for pages, `limit` for offset).
*   **Limit Parameter**: e.g., `limit=50`.
*   **Stop Condition**:
    *   *Empty List*: Stops when the API returns an empty array.
    *   *Field Value*: Stops when `hasMore` is false.

## 4. AI Agent Integration (Tool Mode)

When connected to an **AI Agent** node, the HTTP Request node transforms into a Tool.

### Tool Settings
*   **Name**: The function name the LLM sees (e.g., `get_weather`).
*   **Description**: **Critical**. Instructions for the LLM on *when* and *how* to use this tool.
    *   *Bad*: "Gets weather."
    *   *Good*: "Call this to retrieve current weather data. Requires a 'city' parameter (string)."

### Output Optimization (Token Saving)
LLMs have context limits. Dumping a 5MB JSON response will crash the agent or cost a fortune.
*   **Optimize Response**:
    *   **Fields to Extract**: Use JMESPath to pick only what's needed.
        *   Example: `current_weather.temp_c, location.name`
    *   **Text Truncation**: Cut off after 2000 characters.
    *   **HTML to Text**: If scraping a website, strips all HTML tags to return clean text.

## 5. Advanced Patterns

### Batching (Rate Limit Avoidance)
If you need to make 1000 requests but the API limit is 60/min:
1.  **Split In Batches**: Set batch size to `1`.
2.  **HTTP Request**: Perform the call.
3.  **Wait Node**: Set to `1` second.
4.  **Loop**: Connect Wait output back to Split In Batches.

### Proxy & Certificates
*   **Ignore SSL Issues**: Enable in "Options" for internal tools with self-signed certs.
*   **Proxy**: Configure `N8N_PROXY_RULES` environment variable to route traffic through a proxy.

### cURL Import
You can paste a raw cURL command into the node.
1.  Copy cURL from API docs.
2.  Click "Import cURL" in the node.
3.  Paste.
4.  n8n auto-fills Method, URL, Headers, and Body.
