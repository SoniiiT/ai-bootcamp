```markdown
# Core Nodes & Logic

This guide covers the essential nodes for controlling data flow, logic, and execution order in n8n.

## 1. Logic & Branching

### IF Node
The fundamental binary decision maker.
*   **Logic**: Evaluates a condition (True/False).
*   **Outputs**: Has a `True` branch and a `False` branch.
*   **Comparison Types**:
    *   **String**: Equals, Contains, Starts With, Regex.
    *   **Number**: >, <, =, >=, <=.
    *   **Boolean**: Is True/False.
    *   **Date**: Is After, Is Before.
    *   **Empty**: Is Empty / Is Not Empty.
*   **Use Case**: "If email contains '@gmail.com', route to Personal List, else route to Business List."

### Switch Node
Multi-path routing based on rules.
*   **Logic**: Evaluates an input value against multiple rules.
*   **Outputs**: Creates an output 0, 1, 2... for each rule defined.
*   **Routing**:
    *   **First Match**: Routes the item to the *first* rule that matches.
    *   **All Matches**: Routes the item to *all* rules that match (duplicating the item).
*   **Fallback**: Can define a fallback output for items that match no rules.
*   **Use Case**: "Route support tickets based on 'Priority' field: High -> Slack, Medium -> Email, Low -> Database."

## 2. Merging Data

The **Merge Node** is critical for bringing split workflows back together or joining datasets.

### Modes
1.  **Append**:
    *   **Behavior**: Stacks items from Input 2 *after* items from Input 1.
    *   **Result**: One long list.
    *   **Use Case**: Combining two lists of leads from different sources.
2.  **Merge by Key (Join)**:
    *   **Behavior**: Like an SQL JOIN. Matches items from Input 1 and Input 2 based on a common field (e.g., `email`).
    *   **Types**:
        *   *Inner Join*: Keep only matches.
        *   *Left Join*: Keep all from Input 1, add data from Input 2 if found.
    *   **Use Case**: Enriching a list of User IDs with User Details from a separate API call.
3.  **Merge by Position**:
    *   **Behavior**: Merges Item 1 from Input 1 with Item 1 from Input 2.
    *   **Use Case**: When you know the order is identical (rarely safe).
4.  **Wait (Pass Through)**:
    *   **Behavior**: Waits for *both* inputs to finish executing before proceeding. Passes data from *one* chosen input.
    *   **Use Case**: Ensuring two parallel tasks (e.g., "Create User" and "Create Folder") are both done before sending a "Welcome Email".

## 3. Looping & Rate Limiting

n8n loops over items automatically for most nodes. However, for **Rate Limiting** or **Sequential Processing**, you must use the **Split In Batches** node.

### The "Split In Batches" Pattern
This is the standard pattern for processing large lists or rate-limited APIs.

**Structure:**
1.  **Split In Batches**:
    *   *Batch Size*: `1` (for strict rate limits) or `10` (for bulk APIs).
2.  **Processing Nodes**:
    *   Connect to the **Loop** output of Split In Batches.
    *   Perform actions (e.g., HTTP Request).
3.  **Wait Node** (Optional):
    *   Add a delay (e.g., `1 second`) to respect API limits.
4.  **Loop Back**:
    *   Connect the output of the last node in the chain *back* to the **input** of Split In Batches.

**How it works**:
*   The node takes the first batch.
*   Processes it.
*   The connection back triggers the Split In Batches node to release the *next* batch.
*   When no items remain, the **Done** output fires.

## 4. Data Manipulation

### Edit Fields (Set)
The primary node for simple data transformation.
*   **Operations**:
    *   **Set**: Create or overwrite a field.
    *   **Remove**: Delete a field.
    *   **Keep Only**: Delete all fields *except* the specified ones.
*   **Values**: Can be static strings, numbers, or Expressions (`{{ $json.field }}`).

### Aggregate
Groups multiple items into a single item.
*   **Modes**:
    *   **Aggregate All**: Turns 10 items into 1 item containing a list.
    *   **Group By**: Groups items by a specific field (e.g., "Category").
*   **Use Case**: Creating a summary email ("Here are the 5 tasks completed today").

### Limit
Reduces the number of items.
*   **Use Case**: Testing workflows (only process the first 5 items).

## 5. Utilities

### Wait
Pauses execution.
*   **Resume On**:
    *   **Time**: After X seconds/minutes/hours.
    *   **Date**: At a specific timestamp.
    *   **Webhook**: Generates a unique "Resume URL". The workflow pauses until this URL is called (e.g., by an external approval system).

### Execute Command
Runs shell commands on the host OS.
*   **Availability**: Self-hosted only.
*   **Security**: Dangerous. Ensure inputs are sanitized.
*   **Use Case**: Running a Python script, moving files on disk, invoking `ffmpeg`.

### No Op (No Operation)
Does nothing.
*   **Use Case**: A placeholder for aligning layout or merging branches where no action is needed.

## 6. Error Handling (Core)

### Stop Workflow
Forces the workflow to stop.
*   **Use Case**: In an IF node, if a critical validation fails, route to Stop Workflow.

### Error Trigger
A special Trigger node that runs *only* when another workflow fails.
*   **Use Case**: Creating a global "Error Handler" workflow that sends alerts to Slack/Email whenever any production workflow crashes.
```