# Rule 1 - n8n Workflow Generation

## Purpose
Ensures that n8n workflows are generated in a format that is directly usable (Copy & Paste), secure, and technically accurate according to n8n's specific architecture.

## Core Requirements

### 1. Output Format: JSON Copy & Paste
**Description:** The primary output for the workflow logic must be the n8n JSON format.
**Implementation:**
- Provide the workflow code in a JSON code block.
- Ensure the JSON structure is valid (contains `nodes` and `connections`).
- **User Instruction**: Explicitly tell the user: "You can copy this JSON code and paste it directly into your n8n editor (Ctrl+V)."

**Example:**
```json
{
  "nodes": [
    {
      "parameters": {},
      "name": "Start",
      "type": "n8n-nodes-base.start",
      "typeVersion": 1,
      "position": [250, 300]
    }
  ],
  "connections": {}
}
```

### 2. Technical Accuracy & Data Structure
**Description:** You must strictly adhere to n8n's internal data structure and best practices defined in the context files.
**Implementation:**
- **Code Node**: Consult `code-node.md`.
    - **Input**: Use `$input.all()` (v1 style) or `items` (legacy).
    - **Output**: ALWAYS return an array of objects wrapped in the `json` key: `return [{ json: { myField: "value" } }];`.
    - **Dates**: Use Luxon (`DateTime`, `$now`) for all date math. Do NOT use native `Date` object unless necessary.
- **Expressions**: Consult `expressions.md`.
    - **Syntax**: Use `{{ $json.field }}`.
    - **Looping**: Use the "Paired Item" syntax `{{ $('Node').item.json.id }}` when referencing nodes inside a loop/merge to ensure correct item mapping.
- **AI Agents**: Consult `ai-advanced.md`.
    - **Architecture**: Use the **AI Agent** node as the root.
    - **Connections**: You MUST explicitly connect the sub-nodes (Chat Model, Memory, Tools) to the specific **Input** connectors of the Agent node in the JSON.
- **Binary Data**: Consult `binary-data.md`.
    - **Handling**: Use `binary` property. Use `Move Binary Data` node to convert to/from JSON.
- **Error Handling**: Consult `error-handling.md`.
    - **Resilience**: For external API calls, configure "Continue On Fail" and check for `error` output, or suggest a Global Error Workflow.

### 3. Production Readiness
**Description:** Workflows should be designed for stability and scale, not just to "work once".
**Implementation:**
- **Environment Variables**: Consult `production.md`. Use `{{ $env.MY_VAR }}` for secrets/config instead of hardcoded strings.
- **State Management**: Consult `platform.md`. Use `staticData` for polling triggers to avoid processing duplicates.
- **Rate Limiting**: Consult `integrations.md`. Use "Split In Batches" + "Wait" node for rate-limited APIs. Do NOT create tight loops without delays.
- **Optimization**: Consult `workflows.md`. Advise users to disable "Save Execution Progress" for high-volume workflows.

### 4. Handling Credentials & APIs
**Description:** Credentials must NEVER be part of the JSON output.
**Implementation:**
- **Credentials**: Consult `credentials.md`. Explain exactly which type to use (Predefined vs Generic).
- **Integrations**: Consult `integrations.md`. Prefer Built-in Nodes. Use "Custom API Call" within built-in nodes for missing operations before switching to HTTP Request.
- **HTTP Requests**: Consult `http-request.md`. Use correct authentication and pagination settings.
- **In Text**: Provide a **Step-by-Step Guide** separate from the code block explaining how to set up the credentials.
- **Instruction**: "Please create a new Credential of type '[Type]' and enter your API Key."

### 5. Node Configuration
- **Code Node**: If complex logic is needed, prefer the `Code` node (JavaScript) over multiple complex IF/Switch nodes if it simplifies the view.
- **Naming**: Give nodes meaningful names (e.g., "Get Customer Data" instead of "HTTP Request").
- **Layout**: If generating positions, try to arrange them logically (left to right).

## Response Structure for n8n Requests

1. **Workflow Overview**: Brief explanation of what the flow does.
2. **Prerequisites**: List of needed Credentials (e.g., "You need a Slack API Token").
3. **The Workflow (JSON)**: The copy-pasteable code block.
4. **Setup Instructions**:
   - How to configure the Credentials.
   - Any specific environment variables or settings.
   - **Testing**: Suggest using "Pin Data" or a "Manual Trigger" to test the flow safely.
