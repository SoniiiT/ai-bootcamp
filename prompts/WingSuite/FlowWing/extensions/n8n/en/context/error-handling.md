---
description: Comprehensive guide to handling errors, debugging, and building robust workflows in n8n.
globs: **/*.json
---

# Error Handling & Debugging Guide

Building robust workflows means assuming things *will* go wrong. n8n provides a multi-layered approach to error handling, from simple retries to complex global error handlers.

## 1. The Three Levels of Defense

### Level 1: Node Retries (Transient Errors)
Best for network blips (e.g., API timeout, 503 Service Unavailable).
*   **Mechanism**: The node tries again automatically before failing.
*   **Configuration**: In Node Settings -> **Retry On Fail**.
    *   *Max Tries*: e.g., 3.
    *   *Wait Between Tries*: e.g., 5000ms.
*   **Supported Nodes**: HTTP Request, and many built-in integration nodes.

### Level 2: Node "Continue On Fail" (Logic Errors)
Best for expected failures (e.g., "User not found", "File missing") where the workflow should decide what to do next.
*   **Mechanism**: The node fails, but the workflow continues. The node outputs error metadata.
*   **Configuration**: In Node Settings -> **On Error** -> **Continue**.
*   **Output**: The node adds an `error` object to the output JSON.
    ```json
    {
      "json": {
        "error": {
          "message": "404 - Not Found",
          "description": "The resource could not be found."
        }
      }
    }
    ```

### Level 3: Global Error Workflow (Catastrophic Errors)
Best for unhandled crashes (e.g., OOM, Syntax Error, Database Down).
*   **Mechanism**: If a workflow crashes, n8n triggers a separate "Error Workflow".
*   **Usage**: Sending alerts (Slack/PagerDuty) to the dev team.

---

## 2. Implementing the "Try-Catch" Pattern

n8n doesn't have a native `Try-Catch` block, but you can simulate it using **Continue On Fail**.

### The Pattern
1.  **Action Node** (The "Try"):
    *   Enable **Continue On Fail**.
2.  **IF Node** (The "Catch"):
    *   **Condition**: `{{ $json.error }}` *Is Not Empty*.
3.  **True Branch** (Error Handler):
    *   Handle the error (e.g., Create User if "User Not Found" error).
4.  **False Branch** (Success):
    *   Continue normal flow.

---

## 3. The Global Error Workflow

This is mandatory for production. It catches anything you missed in Level 1 and 2.

### Setup
1.  Create a new workflow named **"Global Error Handler"**.
2.  **Trigger**: Add an **Error Trigger** node.
3.  **Action**: Add a messaging node (Slack, Teams, Email).
4.  **Configure Message**:
    *   **Workflow**: `{{ $execution.workflow.name }}`
    *   **Execution ID**: `{{ $execution.id }}`
    *   **Error Message**: `{{ $execution.error.message }}`
    *   **Link**: `{{ $execution.url }}` (Direct link to the failed execution UI).
5.  **Link**: Go to your **Main Workflow** -> Settings -> **Error Workflow** -> Select "Global Error Handler".

### What Data is Available?
The **Error Trigger** node outputs rich context:
*   `execution`: ID, URL, Mode (manual/production).
*   `workflow`: ID, Name.
*   `error`: Message, Stack Trace, Node Name that failed.

---

## 4. Sub-workflow Error Propagation

When using `Execute Workflow`, errors behave differently.

### Default Behavior
If the Sub-workflow fails, the Parent workflow fails immediately at the `Execute Workflow` node. The Global Error Handler of the **Parent** is triggered.

### Bubbling Errors (Graceful Failure)
If you want the Parent to handle the Child's error logically (e.g., "If Child fails, try Child B"):

1.  **In Sub-workflow**:
    *   Add an **Error Trigger** node.
    *   Connect it to a **Set** node.
    *   Output: `{ "success": false, "error": "Something went wrong" }`.
    *   **Crucial**: This makes the Sub-workflow finish "successfully" (technically), but returning an error object.
2.  **In Parent Workflow**:
    *   The `Execute Workflow` node will output the `{ "success": false }` object.
    *   Use an **IF** node to check `success`.

---

## 5. Advanced Patterns

### Manual Retry Loop (Wait + Goto)
For nodes that don't support native "Retry On Fail" (e.g., Code Node logic).

1.  **Node A**: The operation.
2.  **IF Node**: Did it succeed?
    *   **Yes**: Continue.
    *   **No**:
        1.  **Wait Node**: Wait 5 seconds.
        2.  **Connect** the Wait node back to the input of **Node A**.
*   **Warning**: This creates an infinite loop if it never succeeds. Add a counter in a Code node to limit retries (e.g., max 5).

### The "Stop and Error" Node
Sometimes you want to intentionally crash the workflow to trigger the Global Error Handler.
*   **Use Case**: Validation. "If `amount` is negative, this is a critical data corruption. Stop everything."
*   **Node**: **Stop and Error**.
*   **Config**: Set the error message. This message will be sent to the Error Workflow.

### Circuit Breaker (Static Data)
Prevent a broken workflow from spamming your Slack 10,000 times.

```javascript
// Code Node at start of Error Workflow
const staticData = getWorkflowStaticData('global');
const lastAlert = staticData.lastAlertTime || 0;
const now = new Date().getTime();

// Only alert once every 30 minutes
if (now - lastAlert < 1800000) {
  return []; // Stop execution (no alert)
}

staticData.lastAlertTime = now;
return items;
```

---

## 6. Debugging Tips

### Pinning Data
*   **Scenario**: You are building the middle of a workflow and don't want to re-trigger the webhook every time.
*   **Action**: Run the workflow once. Click the node before the one you are building. Click **"Pin Data"**.
*   **Result**: You can now execute the downstream nodes using the saved data instantly.

### Debugging Webhooks
*   **Webhook Test URL**: `.../webhook-test/...`
    *   Waits for you to click "Execute" in the UI.
    *   Shows the data in the UI immediately.
*   **Webhook Prod URL**: `.../webhook/...`
    *   Runs in the background.
    *   Data only visible in "Executions" list.

### The "No Op" Node
*   n8n has a **No Operation, do nothing** node.
*   **Use Case**: Use it as a placeholder to merge branches or temporarily disconnect parts of a flow without deleting connections.

