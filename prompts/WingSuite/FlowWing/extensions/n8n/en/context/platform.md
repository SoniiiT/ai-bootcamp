---
description: Comprehensive guide to the n8n platform architecture, data structures, and execution logic.
globs: **/*.json
---

# n8n Platform Architecture Guide

Understanding how n8n thinks is key to building robust workflows. This guide covers the internal data structure, execution flow, and state management.

## 1. The Data Structure

n8n is opinionated about data. Every node receives and outputs data in a specific format.

### The "Item"
The fundamental unit of data is an **Item**. An item is an object with two main properties:
1.  **`json`**: A standard JavaScript object containing your data fields.
2.  **`binary`**: An object containing binary file references (optional).

### The "Input Array"
Nodes do not process single items; they process an **Array of Items**.
Even if you only have one piece of data, it is wrapped in an array.

```javascript
[
  {
    "json": {
      "id": 1,
      "name": "Alice"
    },
    "binary": {}
  },
  {
    "json": {
      "id": 2,
      "name": "Bob"
    },
    "binary": {}
  }
]
```

### Implication: Implicit Looping
Because input is always an array, **most nodes loop automatically**.
*   If you connect a "Google Sheets Read" (returning 100 rows) to a "Slack Send Message" node, the Slack node will run **100 times**, once for each item.
*   **You do not need a loop node** for standard 1-to-1 processing.

---

## 2. Execution Logic

### Node Execution
n8n executes nodes in a topological order (dependency graph).
1.  **Start**: The Trigger node runs and produces an array of items.
2.  **Next**: The next node receives that array.
3.  **Processing**: The node processes the items (either all at once or one by one, depending on the node type).
4.  **Output**: The node outputs a new array of items for the next node.

### Branching (IF / Switch)
*   **Splitting**: When an `IF` node runs, it splits the input array into two arrays: `True` and `False`.
*   **Parallel Execution**: Both branches can execute if items go down both paths.

### Merging
To bring branches back together, you **must** use a **Merge** node.
*   **Append**: Simply stacks items from Input 1 and Input 2 into a single array.
*   **Wait (Pass-through)**: Waits for Input 1 AND Input 2 to finish, then outputs Input 1 (ignoring Input 2's data, but using it as a signal).
*   **Keep Key Matches**: Joins items like a SQL `JOIN` based on a shared field (e.g., `email`).
*   **Multiplex**: Creates a Cartesian product (every item from Input 1 combined with every item from Input 2).

---

## 3. Looping (Explicit)

While implicit looping handles 1-to-1 tasks, you need the **Loop Over Items** node for:
1.  **Batching**: Sending 10 items at a time to an API to respect rate limits.
2.  **Resetting Context**: If you need to calculate a sum *per item* using a sub-workflow.
3.  **Fan-out**: If one item needs to generate multiple output items that need distinct processing.

### Configuration
*   **Batch Size**: How many items to process in one iteration (default 1).
*   **Loop End**: You must connect the end of the loop back to the **Loop Over Items** node to signal completion of a batch.

---

## 4. State Management

### Workflow Static Data
n8n is stateless by default. To remember data *between* executions (e.g., "Last Polled ID"), use **Static Data**.

*   **Scope**:
    *   `global`: Shared across the entire workflow.
    *   `node`: Scoped to a specific node (used by polling triggers).
*   **Usage (Code Node)**:
    ```javascript
    const staticData = getWorkflowStaticData('global');
    const lastId = staticData.lastId || 0;
    
    // Update logic
    staticData.lastId = 123;
    ```
*   **Important**: Static data is **only saved** when the workflow runs in **Production** mode (via a Trigger). It is NOT saved during manual testing in the UI.

### Variables (`$vars`)
*   Defined in **Settings > Variables**.
*   Read-only during execution.
*   Good for constants like `API_BASE_URL` or `SLACK_CHANNEL_ID`.

---

## 5. AI Architecture (LangChain)

n8n's AI implementation differs from standard nodes.

### The "Cluster" Concept
*   **Root Node**: The **AI Agent** or **Chain**. This is the only node that "executes" in the standard flow.
*   **Sub-Nodes**: Nodes like **Chat Model**, **Memory**, and **Tools** connect to the Root Node via *special inputs* (grey dots), not the main output.
*   **Execution**: When the Agent runs, it "pulls" capabilities from the sub-nodes. The sub-nodes do not run independently; they are configuration providers for the Agent.

---

## 6. Best Practices

1.  **Minimize Loops**: Explicit loops are slow. Use implicit looping (batch processing) whenever possible.
2.  **Filter Early**: Use `IF` nodes early to remove unnecessary items and save processing time.
3.  **Merge Correctly**: If you split a flow, always Merge it back if you need the data later. Leaving dangling branches can cause unexpected behavior.
4.  **Naming**: Rename nodes immediately. `HTTP Request 15` is impossible to debug. Use `Get User Data`.
