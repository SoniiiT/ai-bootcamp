---
description: A collection of advanced patterns, recipes, and code snippets for n8n.
globs: **/*.json
---

# n8n Cookbook & Recipes

This guide provides copy-pasteable solutions for common and advanced n8n challenges.

## 1. Advanced Pagination

### Scenario: Offset-based Pagination
The API requires `?limit=50&offset=0`, then `offset=50`, etc.

**Pattern**:
1.  **Set Node**: Initialize `offset = 0` and `limit = 50`.
2.  **Loop Node (Code)**: A custom loop logic.
    *   *Better Way*: Use the **HTTP Request** node's built-in pagination (available in newer versions).
    *   *Manual Way*:
        1.  **HTTP Request**: Use `{{ $node["Set"].json.offset }}`.
        2.  **IF Node**: `{{ $json.length < 50 }}`?
            *   **True**: End.
            *   **False**: **Set Node** (`offset = offset + 50`) -> Connect back to HTTP Request.

### Scenario: Link Header Pagination
The API returns the next URL in the `Link` header (e.g., GitHub).

**Pattern**:
1.  **HTTP Request**: Enable "Pagination".
2.  **Pagination Mode**: "Response Header".
3.  **Header Name**: `link` or `Link`.
4.  **Next URL Rule**: n8n usually parses standard RFC 5988 Link headers automatically.

---

## 2. Error Handling Patterns

### The "Try-Catch" Block
n8n doesn't have a native Try-Catch block, but you can simulate it.

**Pattern**:
1.  **Node A (The "Try")**: Enable "Continue On Fail".
2.  **IF Node**: Check `{{ $json.error !== undefined }}`.
    *   **True (Catch)**: Route to error handling logic (Slack alert, DB log).
    *   **False (Success)**: Continue normal flow.

### The "Global Error Handler"
**Pattern**:
1.  Create a separate workflow named "Error Handler".
2.  **Trigger**: Error Trigger node.
3.  **Action**: Send Slack message with:
    *   Workflow Name: `{{ $workflow.name }}`
    *   Execution ID: `{{ $execution.id }}`
    *   Error Message: `{{ $execution.error.message }}`
    *   Link: `{{ $execution.url }}`
4.  **Setup**: In every production workflow, go to Settings -> Error Workflow -> Select "Error Handler".

---

## 3. Data Deduplication

### Scenario: Polling an API that doesn't support filtering by date
You fetch 100 items, but only want the new ones since the last run.

**Pattern**:
1.  **Code Node**:
    ```javascript
    const staticData = getWorkflowStaticData('node');
    const lastId = staticData.lastId || 0;
    const newItems = items.filter(item => item.json.id > lastId);
    
    if (newItems.length > 0) {
      // Update lastId to the max ID found
      staticData.lastId = Math.max(...newItems.map(i => i.json.id));
    }
    
    return newItems;
    ```

---

## 4. Dynamic Webhook Responses

### Scenario: Return 200 OK immediately, then process
You want to acknowledge a webhook instantly to prevent timeouts, then do heavy processing.

**Pattern**:
1.  **Webhook Node**:
    *   **Response Mode**: "On Received".
    *   *Result*: Returns 200 OK instantly.
2.  **Processing Nodes**: Do your heavy lifting (PDF generation, AI, etc.).
    *   *Note*: The caller will NOT receive the result of the processing.

### Scenario: Return conditional status codes
Return 400 if validation fails, 200 if success.

**Pattern**:
1.  **Webhook Node**:
    *   **Response Mode**: "Last Node".
2.  **IF Node**: Validate input.
    *   **True**: **Respond to Webhook** node (Code: 200, Body: `{ "status": "ok" }`).
    *   **False**: **Respond to Webhook** node (Code: 400, Body: `{ "error": "Invalid Input" }`).

---

## 5. File Processing

### Scenario: Unzip, Process, Re-zip
**Pattern**:
1.  **Read Binary File**: Load `archive.zip`.
2.  **Compression Node**: Action "Decompress".
    *   *Output*: Multiple items (one per file in zip).
3.  **Filter**: Keep only `.csv` files.
4.  **Spreadsheet File**: Read CSV.
5.  **... Process Data ...**
6.  **Spreadsheet File**: Write CSV.
7.  **Compression Node**: Action "Compress".
    *   *Input*: All items.
    *   *Output*: Single `new_archive.zip`.

---

## 6. Advanced Code Snippets

### Flattening Nested Arrays
Turn `[{ "users": [A, B] }, { "users": [C, D] }]` into `[A, B, C, D]`.

```javascript
// Code Node
const allUsers = [];
for (const item of items) {
  allUsers.push(...item.json.users);
}
return allUsers.map(user => ({ json: user }));
```

### Generating HMAC Signature
Secure your webhooks by verifying signatures.

```javascript
const crypto = require('crypto');
const secret = 'my-secret-key';
const body = JSON.stringify(items[0].json);
const signature = crypto.createHmac('sha256', secret).update(body).digest('hex');
return [{ json: { signature } }];
```

### Sleep / Delay (Randomized)
Avoid bot detection by sleeping for a random time.

```javascript
// Code Node
const waitTime = Math.floor(Math.random() * 5000) + 1000; // 1-6 seconds
await new Promise(resolve => setTimeout(resolve, waitTime));
return items;
```

---

## 7. Rate Limiting (Token Bucket)

### Scenario: Strict 1 request per second
**Pattern**:
1.  **Loop Over Items**: Batch Size = 1.
2.  **HTTP Request**: Make the call.
3.  **Wait Node**:
    *   **Resume**: "After Time Interval".
    *   **Amount**: 1.
    *   **Unit**: Seconds.
