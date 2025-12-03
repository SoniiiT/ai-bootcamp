# Advanced AI & LangChain Guide

This guide provides a deep dive into building autonomous agents, RAG pipelines, and complex AI workflows using n8n's LangChain integration.

## Core Architecture

n8n's AI capabilities are built on top of **LangChain**. The architecture differs from standard n8n workflows:
*   **Main Node**: The "Brain" (Agent or Chain) that executes the logic.
*   **Sub-Nodes**: "Limbs" that provide capabilities (Models, Memory, Tools, Retrievers). These connect to specific *input ports* on the main node, not the standard workflow output.

---

## AI Agents

Agents use an LLM to reason about *what* to do. They can call tools, observe the output, and decide the next step.

### Agent Types

| Agent Type | Best For | Description |
| :--- | :--- | :--- |
| **Tools Agent** | General Purpose | The most versatile agent. Uses OpenAI's function calling (or similar) to reliably execute tools. Recommended for most use cases. |
| **Conversational Agent** | Chatbots | Optimized for maintaining conversation history. Less reliable with complex tool usage than the Tools Agent. |
| **ReAct Agent** | Non-Function Models | Uses the "Reasoning + Acting" pattern. Good for models that don't support native function calling (e.g., older open-source models). Verbose and slower. |
| **Plan & Execute** | Complex Tasks | Breaks a complex goal into a step-by-step plan, then executes each step. Good for multi-stage research. |

### Configuration Deep Dive

#### 1. System Prompt
The most critical configuration. Define:
*   **Persona**: "You are a senior support engineer..."
*   **Constraints**: "Never make up facts. If you don't know, say so."
*   **Formatting**: "Always answer in Markdown."
*   **Date/Time**: Inject `{{ $now }}` so the agent knows the current context.

#### 2. Memory (Context)
*   **Window Buffer Memory**: Keeps the last K interactions. Best for most chat bots.
*   **Summary Buffer Memory**: Summarizes older interactions to save tokens while keeping recent ones verbatim.
*   **Vector Store Memory**: Stores conversation history in a vector database for long-term recall (infinite memory).
*   **Session Management**: Use the `sessionId` property in the Memory node to isolate conversations between users.
    *   *Pattern*: Set `Session ID` to `{{ $json.chatId }}` (coming from a webhook or chat trigger).

---

## Tools & Capabilities

Tools allow the Agent to interact with the outside world.

### Built-in Tools
*   **Calculator**: Math operations.
*   **Wikipedia / SerpApi**: Web search and knowledge.
*   **Code Tool**: Execute Python/JS code sandboxed.

### Custom Tools (The Powerhouse)
You can turn *any* n8n workflow or HTTP request into a tool.

#### 1. Call n8n Workflow Tool
Delegates a task to another workflow.
*   **Name**: Must be descriptive (e.g., `get_customer_order`).
*   **Description**: **CRITICAL**. The LLM reads this to decide *when* to use the tool. Example: "Call this to retrieve order details given an Order ID."
*   **Schema**: Define the inputs the tool expects (e.g., `orderId` as a string).
*   **Workflow**: The sub-workflow must start with an `Execute Workflow Trigger` and end with a `Respond to Webhook` node (returning the tool output).

#### 2. HTTP Request Tool
Directly calls an API.
*   **Setup**: Similar to the standard HTTP Request node, but triggered by the Agent.
*   **Dynamic Parameters**: Use expressions to map tool inputs to API parameters.

#### 3. Dynamic Tool Inputs (`$fromAI()`)
When defining tool parameters, you often want the LLM to provide the value.
*   **Usage**: In the tool configuration, set a parameter to **Expression** and use `$fromAI('parameterName')`.
*   **Description**: You must provide a description for `parameterName` so the LLM knows what to put there.

---

## RAG (Retrieval Augmented Generation)

RAG allows the AI to answer questions based on your own data (PDFs, Notion, SQL, etc.) without fine-tuning.

### The RAG Pipeline

#### Phase 1: Ingestion (Loading Data)
1.  **Document Loader**: Reads data.
    *   *Nodes*: `Text File`, `PDF Loader`, `Notion`, `Web Scraper`, `Github`.
2.  **Text Splitter**: Chunks data into manageable pieces.
    *   *Node*: `Recursive Character Text Splitter`.
    *   *Config*: `Chunk Size` (e.g., 1000 chars), `Chunk Overlap` (e.g., 200 chars). Overlap is crucial to maintain context between chunks.
3.  **Embeddings**: Converts text to vectors.
    *   *Node*: `OpenAI Embeddings`, `HuggingFace Embeddings`.
4.  **Vector Store (Insert)**: Saves vectors.
    *   *Nodes*: `Pinecone`, `Qdrant`, `Supabase`, `Postgres (PgVector)`.
    *   *Mode*: Select **Insert Documents**.

#### Phase 2: Retrieval (Querying)
1.  **Vector Store (Retrieve)**: Finds relevant chunks.
    *   *Mode*: Select **Retrieve Documents** (or connect as a Retriever to a Chain).
    *   *Query*: Usually comes from the user's question.
2.  **QA Chain**: Synthesizes the answer.
    *   *Node*: `Conversational Retrieval QA Chain`.
    *   *Inputs*: Connect the **Chat Model** and the **Vector Store Retriever**.

### Advanced RAG Techniques
*   **Hybrid Search**: Combine keyword search (BM25) with vector search for better accuracy (supported by Supabase/Qdrant).
*   **Metadata Filtering**: Filter chunks by metadata (e.g., `userId`, `docType`) before retrieval to ensure security and relevance.
*   **Multi-Query Retriever**: Generates multiple variations of the user's question to find better matches.

---

## Chains vs. Agents

| Feature | Chain | Agent |
| :--- | :--- | :--- |
| **Logic** | Hardcoded Sequence | Dynamic Reasoning |
| **Predictability** | High | Lower (can loop or hallucinate) |
| **Cost** | Lower | Higher (multiple LLM calls) |
| **Use Case** | Summarization, Translation, Simple RAG | Complex Research, Multi-step Tasks, Tool Usage |

### Common Chains
*   **Basic LLM Chain**: Simple Prompt -> Completion.
*   **Summarization Chain**: Handles long documents using "Map-Reduce" or "Refine" strategies.
*   **Structured Output Parser**: Forces the LLM to return JSON matching a specific schema (Zod).
    *   *Tip*: Connect this to the "Output Parser" input of a Chain to get reliable data for downstream nodes.

---

## Pro Tips & Troubleshooting

### 1. Debugging Agents
*   **Verbose Logging**: Check the browser console or n8n execution logs to see the "Thought Process" of the agent (Intermediate Steps).
*   **Looping**: If an agent gets stuck calling the same tool, check the tool output. If the tool returns an error, the agent might retry indefinitely. Add error handling in the tool to return a descriptive message like "Error: ID not found" instead of failing.

### 2. Model Selection
*   **GPT-4o / Claude 3.5 Sonnet**: Best for complex Agents and coding tasks.
*   **GPT-3.5-Turbo / GPT-4o-mini**: Good for simple classification, summarization, and high-volume tasks.
*   **Local Models (Ollama)**: Great for privacy, but often struggle with complex function calling. Use the **ReAct Agent** for better results with local models.

### 3. Token Management
*   **Context Window**: Be aware of the model's limit (e.g., 128k tokens).
*   **Truncation**: Use the `Token Splitter` to ensure you don't send too much data to the model.
*   **Cost**: Agents can be expensive. Use `Max Iterations` on the Agent node to prevent infinite loops (default is usually 10-15).

### 4. Structured Output
If you need the Agent to output JSON for the next node in your workflow:
1.  Don't just ask in the prompt.
2.  Use the **Structured Output Parser** connected to the Chain/Agent.
3.  Or, use OpenAI's "JSON Mode" in the Chat Model node configuration.
