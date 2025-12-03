```markdown
# Troubleshooting & Debugging Guide

This guide provides comprehensive strategies for identifying, diagnosing, and fixing issues in n8n workflows, ranging from simple logic errors to complex server-side failures.

## 1. Debugging Tools & Techniques

### Pin Data (The "Time Saver")
Pinning allows you to "freeze" the output data of a node.
*   **Why**: Prevents re-running heavy triggers or expensive API calls during development.
*   **How**:
    1.  Run the workflow once.
    2.  Click the "Pin Data" icon (thumbtack) in the node's output panel.
    3.  Now, when you click "Execute Workflow", n8n starts *after* the pinned node, using the cached data.
*   **Warning**: Remember to unpin before deploying to production if the data needs to be dynamic!

### Execution Inspector
The primary tool for post-mortem analysis.
*   **Access**: Click "Executions" in the left sidebar.
*   **Filters**: Filter by "Error", "Success", or specific Workflow ID.
*   **Deep Dive**:
    *   Click an execution to open it.
    *   Click on *any node* to see the **Input** (what it received) and **Output** (what it produced).
    *   **Binary Data**: You can download files processed in past executions to verify their content.

### Console Logging (Code Nodes)
*   **Browser Console**: When manually executing in the UI, `console.log()` output appears in your browser's Developer Tools (F12 > Console).
    *   *Tip*: Use `console.log('MyVar:', JSON.stringify(myVar))` to see full objects.
*   **Server Logs**: In production (Trigger runs), `console.log()` output goes to the n8n server logs (Docker logs).

### "Debug in Editor"
A powerful feature to fix failed production runs.
1.  Open a failed execution.
2.  Click **"Debug in Editor"**.
3.  n8n loads the *exact data* from that failure into your current editor session.
4.  You can now tweak the node settings and re-run just that step to fix the logic.

## 2. Common Error Messages & Fixes

### "Referenced node is unexecuted"
*   **Context**: You used an expression like `$('Node A').item.json.id`.
*   **Cause**: "Node A" is in a branch that didn't run (e.g., the `False` path of an IF node), or it hasn't run *yet*.
*   **Fix**:
    *   Ensure the logic guarantees "Node A" runs before the current node.
    *   Use a **Merge Node** to bring branches back together.
    *   Use defensive coding: `{{ $('Node A').item ? $('Node A').item.json.id : null }}`.

### "JSON parameter need to be an object"
*   **Context**: Code Node output.
*   **Cause**: You returned an array of strings/numbers/nulls. n8n *requires* objects.
*   **Fix**: Wrap values in `json`: `return myStrings.map(s => ({ json: { value: s } }))`.

### "ECONNRESET" / "ETIMEDOUT" / "502 Bad Gateway"
*   **Context**: HTTP Request Node.
*   **Cause**: The external API is down, slow, or blocking you.
*   **Fix**:
    *   **Retry**: Enable "Retry on Fail" in Node Settings (e.g., 3 retries).
    *   **Timeout**: Increase the "Timeout" setting in the node options.
    *   **Rate Limit**: You might be sending too fast. Use "Split In Batches" + "Wait".

### "Memory Limit Exceeded" / "JavaScript heap out of memory"
*   **Context**: Processing large JSON files or SQL dumps.
*   **Cause**: n8n loads all data into RAM. A 100MB JSON file might take 500MB RAM to parse.
*   **Fix**:
    *   **Split In Batches**: Process 50 items at a time, not 50,000.
    *   **Optimize Fields**: In SQL/HTTP nodes, select *only* the columns you need.
    *   **Sub-workflows**: Offload processing to a worker workflow.

### "No binary data property"
*   **Context**: Uploading files or manipulating images.
*   **Cause**: The node expects a binary property named `data` (default), but your previous node output named it `file` or `attachment`.
*   **Fix**: Check the "Input Data Field Name" setting and match it to the actual incoming property name.

## 3. Error Handling Strategies

### Level 1: "Continue On Error" (Resilience)
*   **Setting**: In Node Settings (Gear Icon) > "On Error" > "Continue".
*   **Behavior**: If the node fails, it outputs a JSON object with `error: "message"`. The workflow continues.
*   **Use Case**: Processing a list of 100 emails. If one fails, you don't want the other 99 to stop.
*   **Pattern**:
    1.  HTTP Request (Continue On Error).
    2.  IF Node: `{{ $json.error }}` exists?
    3.  True: Log error to DB.
    4.  False: Continue processing.

### Level 2: Error Trigger (Global Handler)
*   **Concept**: A "Catch-All" workflow that runs whenever *any* workflow fails.
*   **Setup**:
    1.  Create a new workflow.
    2.  Add an **Error Trigger** node.
    3.  Add a **Slack/Email** node to send the alert.
    4.  In your Main Workflow > Settings > **Error Workflow**, select this new workflow.
*   **Data Available**: The Error Trigger provides:
    *   `workflow.id`, `workflow.name`
    *   `execution.id`, `execution.url`
    *   `node.name` (which node failed)
    *   `error.message`, `error.stack`

### Level 3: Try/Catch Pattern (Local Handler)
*   **Concept**: Handle errors for a specific section of logic.
*   **Implementation**:
    *   n8n doesn't have a native "Try/Catch" block.
    *   Use **Continue On Error** + **IF Node** (as described in Level 1) to simulate this.

## 4. Connectivity Issues (Self-Hosted)

### "Self signed certificate in certificate chain"
*   **Cause**: Your n8n instance is trying to talk to an internal server with a self-signed SSL cert.
*   **Fix**:
    *   **Option A**: In HTTP Request node, set "Ignore SSL Issues" to True.
    *   **Option B**: Set env var `NODE_TLS_REJECT_UNAUTHORIZED=0` (Global, less secure).

### Proxy Configuration
If n8n is behind a corporate firewall:
*   **Env Vars**:
    *   `HTTP_PROXY`, `HTTPS_PROXY`: Standard proxy settings.
    *   `N8N_PROXY_RULES`: Granular control (e.g., bypass proxy for internal IPs).
```