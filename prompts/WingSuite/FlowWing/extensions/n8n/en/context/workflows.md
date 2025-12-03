---
description: Comprehensive guide to n8n Workflow structure, settings, lifecycle, and best practices.
applyTo: "**"
---

# Workflows Guide

A workflow is a collection of nodes connected to automate a process. This guide covers the lifecycle, configuration, and optimization of workflows.

## 1. Workflow Structure

### The Trigger
Every workflow starts with a **Trigger Node**.
*   **Webhook**: Runs when a URL is called.
*   **Schedule**: Runs at a specific time (Cron).
*   **App Trigger**: Runs when an event happens in an app (e.g., "Slack - On New Message").
*   **Manual**: Runs when you click "Execute Workflow" (used for testing or sub-workflows).

### The Flow
*   **Linear**: A -> B -> C.
*   **Branching**: A -> IF -> (B or C).
*   **Looping**: A -> Loop -> B -> Loop.

---

## 2. Workflow Settings

Configuring these correctly is vital for production stability. Access via **Settings** (top right).

### Execution Data (Database Bloat)
*   **Save Execution Progress**: **DISABLE THIS**.
    *   *Why*: If enabled, n8n writes the full JSON/Binary payload to the DB after *every single node*. This kills performance and fills the disk.
    *   *Exception*: Enable only while debugging a specific crash.
*   **Save Data Error Execution**: **Enable**. You need this to debug failures.
*   **Save Data Success Execution**: **Disable** for high-volume workflows.

### Error Handling
*   **Error Workflow**: Select a workflow to run if this one fails.
    *   *Usage*: Send a Slack alert with the execution ID and error message.
*   **Timeouts**: Set a **Timeout After** (e.g., 1 hour).
    *   *Why*: Prevents "zombie" executions from running forever if an API hangs.

### Timezone
*   **Default**: Uses the instance timezone (`GENERIC_TIMEZONE`).
*   **Override**: Set a specific timezone if your schedule depends on a specific region (e.g., "9 AM Tokyo Time").

---

## 3. Activation & Lifecycle

### Active vs Inactive
*   **Inactive**: Triggers are disabled. Webhooks return 404 (unless using the Test URL).
*   **Active**: Triggers are live.

### Webhook URLs
*   **Test URL**: `https://.../webhook-test/...`
    *   Only works when you have the UI open and click "Execute Workflow".
    *   Good for debugging.
*   **Production URL**: `https://.../webhook/...`
    *   Only works when the workflow is **Active**.
    *   Does not show data in the UI immediately (check Execution History).

---

## 4. Error Handling Strategies

### Node Level
*   **Continue On Fail**: Enable this in a node's settings to prevent the workflow from stopping.
*   **Error Output**: When "Continue On Fail" is on, the node adds an `error` property to the output. Use an **IF** node to check for it.

### Workflow Level
*   **Error Trigger Node**: Create a separate "Error Handler" workflow.
    *   Trigger: **Error Trigger**.
    *   Action: Send Email/Slack.
    *   Config: Set this workflow as the "Error Workflow" in your main workflows.

---

## 5. Debugging & Development

### Pinning Data
You can "Pin" data to a node to simulate input without re-running the trigger.
1.  Run the workflow once to get data.
2.  Click the node -> "Pin Data".
3.  Now you can edit downstream nodes and run them instantly using the pinned data.

### Execution History
*   **Filter**: View only "Error" executions.
*   **Retry**: You can retry a failed execution from the UI.
    *   *Note*: It retries with the *original* data. If you fixed a bug in the workflow, the retry might still fail if the logic hasn't been applied to the old execution context (depending on where it failed).

---

## 6. Concurrency & Limits

### Global Limit
*   `N8N_CONCURRENCY_PRODUCTION_LIMIT`: Max active executions.
*   If exceeded, new executions are queued (or rejected if queue is full).

### Workflow Limit
*   You cannot strictly limit concurrency per workflow in the UI.
*   **Workaround**: Use a **Redis** or **Postgres** lock in a Code node at the start of the workflow if you need to ensure singleton execution.

---

## 7. Best Practices

1.  **Idempotency**: Design workflows so they can be retried safely.
    *   *Bad*: "Create User". (Retry = Duplicate User).
    *   *Good*: "Check if User Exists -> Create if not".
2.  **Annotations**: Use **Notes** (Sticky Notes) on the canvas to explain complex logic.
3.  **Clean Layout**: Use "Auto-align" to keep the graph readable.
4.  **Naming**: Name your routes in Switch nodes (e.g., "Is Customer" vs "Is Lead").
