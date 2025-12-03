```markdown
---
description: Guide to extending n8n with custom nodes, API actions, and LangChain code
---

# Custom Extensions & Advanced Code

When built-in nodes are insufficient, n8n provides mechanisms to extend functionality through custom API calls, specialized code nodes, and custom node development.

## 1. Custom API Actions (The "Missing Node" Fix)

If a node exists for a service (e.g., Slack) but lacks a specific operation (e.g., "Admin Archive Channel"), or if no node exists but the service has an API, use the **HTTP Request** node.

### Using Predefined Credentials
You can use credentials from existing nodes in a generic HTTP Request. This is the **#1 Best Practice** for extending n8n.

1.  **Add HTTP Request Node**.
2.  **Authentication**: Select **Predefined Credential Type**.
3.  **Credential Type**: Search for the service (e.g., "Google Calendar OAuth2 API").
    *   *Note*: Even if the node is called "Google Calendar", the credential might be named "Google Calendar OAuth2 API".
4.  **Credential**: Select your existing authenticated account.
5.  **URL**: Enter the specific API endpoint from the service's documentation.

**Benefit**: n8n handles the OAuth2 token refresh, header injection, and security. You just provide the URL and Body.

### Credential-Only Nodes
Some integrations are "Credential-only". They appear in the nodes panel but only provide credentials for use with the HTTP Request node.
*   **Examples**: Carbon Black, certain AWS services.
*   **Usage**: Install the node -> Create Credential -> Use in HTTP Request.

## 2. LangChain Code Node

The **LangChain Code Node** is a specialized version of the Code Node designed specifically for the AI Agent ecosystem. It allows you to import LangChain functionality that n8n hasn't wrapped yet.

### Key Differences from Standard Code Node
*   **Language**: Supports **JavaScript only** (no Python).
*   **Context**: Designed to work within an AI Chain or Agent.
*   **Inputs/Outputs**: Can have multiple specialized inputs (e.g., `ai_memory`, `ai_tool`).

### Modes
1.  **Execute**:
    *   **Behavior**: Acts like a standard Code node. Takes input, processes it, returns output.
    *   **Requirement**: Must have a **Main Input** and **Main Output**.
    *   **Use Case**: Custom logic inside a chain (e.g., formatting a prompt before sending to LLM).
2.  **Supply Data**:
    *   **Behavior**: Acts as a sub-node (tool) for a root node (Agent/Chain).
    *   **Requirement**: Uses **non-main outputs** (e.g., connecting to the `Tool` input of an Agent).
    *   **Use Case**: Creating a custom Tool using JavaScript that the Agent can call.

### Built-in Methods
*   `this.addInputData(inputName, data)`: Mock data for non-main inputs.
*   `this.getInputData(inputIndex?, inputName?)`: Get data from main input.
*   `this.getExecutionCancelSignal()`: Handle cancellation in custom chains.
*   `this.loadModule(moduleName)`: (Self-hosted only) Load external LangChain modules if enabled.

## 3. Creating Custom Nodes

If you need reusable logic across many workflows or want to share with the community, build a Custom Node.

### Node Styles

#### A. Declarative Style (JSON)
*   **Best For**: Simple API wrappers (REST/HTTP).
*   **Language**: JSON.
*   **Structure**:
    *   `properties`: Define UI fields (API Key, Resource, Operation).
    *   `routing`: Define how fields map to API requests (Method, URL, Body).
*   **Pros**: Easy to build, no TypeScript knowledge needed, n8n handles UI/Execution.
*   **Cons**: Limited logic. Cannot handle complex data transformations or non-HTTP protocols.

#### B. Programmatic Style (TypeScript)
*   **Best For**: Complex logic, data transformation, or non-HTTP protocols (TCP, SSH).
*   **Language**: TypeScript.
*   **Structure**:
    *   `description`: UI definition.
    *   `execute()`: The main function. You have full control.
        *   Read inputs: `this.getNodeParameter('myField')`.
        *   Process data.
        *   Return JSON.
*   **Pros**: Unlimited power. Can use npm packages.
*   **Cons**: Requires coding skills and build step.

### Development Workflow
1.  **Scaffold**: Use `n8n-node-dev` CLI.
    *   `npx n8n-node-dev new`
2.  **Build**: `npm run build`
3.  **Link**: `npm link` to your local n8n instance for testing.
4.  **Publish**: `npm publish` to npm registry.

### Community Nodes
Custom nodes published to npm can be installed into any n8n instance.
*   **Installation**: Settings > Community Nodes > Install.
*   **Naming**: Must start with `n8n-nodes-`.

## 4. External Secrets (Vaults)

For enterprise security, do not store credentials in the n8n database. Use an External Secret Store.

### Supported Providers
*   AWS Secrets Manager
*   Azure Key Vault
*   Google Cloud Secret Manager
*   HashiCorp Vault
*   Infisical

### Usage
1.  **Connect**: Settings > External Secrets.
2.  **Reference**: In any Credential field (e.g., "API Key"), switch the mode to **Expression**.
3.  **Syntax**: `{{ $secrets.vaultName.secretName }}`.
    *   *Example*: `{{ $secrets.awsSecretsManager.stripe_api_key }}`.

This ensures that if your n8n backup is leaked, the actual keys are not exposed.
```