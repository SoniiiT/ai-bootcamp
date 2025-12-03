---
description: Comprehensive guide for modularizing workflows using Sub-workflows in n8n.
globs: **/*.json
---

# Modularization & Sub-workflows Guide

As n8n projects grow, monolithic workflows become hard to maintain. Modularization involves breaking logic into reusable **Sub-workflows**.

## Core Components

### 1. The Caller: `Execute Workflow` Node
This node sits in the **Parent Workflow** and invokes the sub-workflow.

#### Execution Modes
*   **Run Once for All Items (Batch Mode)**:
    *   **Behavior**: Sends all input items as a single array to the sub-workflow. The sub-workflow runs *once*.
    *   **Use Case**: Aggregation (e.g., "Create a daily report from these 100 rows"), or when the sub-workflow handles its own looping.
    *   **Performance**: High. Preferred for high-volume data.
*   **Run for Each Item (Loop Mode)**:
    *   **Behavior**: The sub-workflow runs *separately* for every single input item.
    *   **Use Case**: Isolation. If Item 5 fails, Items 1-4 and 6-10 still succeed. Complex logic per item.
    *   **Performance**: Low. Can cause overhead if processing thousands of items.

### 2. The Callee: `Execute Workflow Trigger` Node
This node replaces the standard trigger (Webhook/Schedule) in the **Sub-workflow**.
*   **Input**: Receives the JSON and Binary data passed from the parent.
*   **Output**: Starts the execution flow.

---

## Data Passing

### Passing Data Down (Parent -> Child)
*   **JSON**: Whatever JSON enters the `Execute Workflow` node is available in the `Execute Workflow Trigger` node.
*   **Variables**: Sub-workflows **DO NOT** inherit variables or environment data from the parent automatically. You must pass them explicitly as JSON fields.
    *   *Pattern*: Use a `Set` node before the `Execute Workflow` node to add config fields (e.g., `{ "operation": "create_user", "dryRun": true }`).

### Passing Data Up (Child -> Parent)
*   **Implicit Return**: The output of the **last executed node** in the sub-workflow is returned to the parent.
*   **Explicit Return**: It is best practice to end your sub-workflow with a `Set` node named "Output" to clearly define what is returned.
    *   *Example*: `{ "success": true, "id": 123 }`

---

## Error Handling in Sub-workflows

When a sub-workflow fails, the behavior depends on the `Execute Workflow` node settings.

### Default Behavior
If the sub-workflow errors, the `Execute Workflow` node in the parent fails, stopping the parent workflow.

### "Continue On Fail" Pattern
To make the parent robust:
1.  **Parent**: Enable "Continue On Fail" in the `Execute Workflow` node settings.
2.  **Child**: Use an **Error Trigger** node inside the sub-workflow.
    *   Connect the Error Trigger to a `Set` node that outputs `{ "error": true, "message": "..." }`.
    *   This allows the sub-workflow to "fail gracefully" and return an error object instead of crashing the parent.

---

## Common Patterns

### 1. The "Router" Workflow
A main workflow acts as a traffic controller.
1.  **Trigger**: Webhook receives a request.
2.  **Switch Node**: Checks `body.type`.
3.  **Routes**:
    *   Type "order": Call `Sub-workflow: Process Order`.
    *   Type "refund": Call `Sub-workflow: Process Refund`.
    *   Type "support": Call `Sub-workflow: Create Ticket`.

### 2. The "Utility" Workflow
A reusable function used by many workflows.
*   *Examples*: "Enrich Company Data", "Send Slack Notification (Standardized)", "Log to Splunk".
*   *Benefit*: Change the logging logic in one place, and all 50 workflows update instantly.

### 3. Recursive Workflows (Pagination)
n8n allows a workflow to call itself. This is useful for paginating APIs that don't support standard methods.
1.  **Trigger**: Receives `page` number (default 1).
2.  **HTTP Request**: Fetches data for `page`.
3.  **IF Node**: Is there more data?
    *   **Yes**: Call `Execute Workflow` (Self) with `page + 1`.
    *   **No**: Finish.

---

## Testing & Debugging

### Testing Sub-workflows Independently
You cannot easily "Run" a sub-workflow from the UI if it depends on data from a parent.
*   **Solution**: Add a **Manual Trigger** node alongside the `Execute Workflow Trigger`.
*   **Mock Data**: In the Manual Trigger, configure "Test Input" JSON that mimics what the parent would send. This allows you to develop and test the sub-workflow in isolation.

### Debugging
*   **Execution List**: When you view the execution of a Parent workflow, n8n provides a link to the specific execution ID of the Sub-workflow. Click it to see exactly what happened inside the child.

---

## Best Practices

1.  **Naming**: Prefix sub-workflows clearly (e.g., `[Sub] Process User`, `[Util] Send Email`).
2.  **Documentation**: Use the "Notes" feature in the `Execute Workflow Trigger` to document the expected input schema (e.g., "Requires `userId` and `email`").
3.  **Atomic Logic**: Keep sub-workflows focused on a single task.
4.  **Avoid Deep Nesting**: Calling a sub-workflow that calls another sub-workflow is fine, but going 5 levels deep makes debugging a nightmare. Keep it flat if possible.
