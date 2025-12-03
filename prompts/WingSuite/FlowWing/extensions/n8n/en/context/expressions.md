---
description: Comprehensive guide to using Expressions in n8n for dynamic data mapping.
applyTo: "**"
---

# Expressions Guide

Expressions are the glue that holds n8n workflows together. They allow you to dynamically map data from one node to another using JavaScript.

## Core Syntax

Expressions are wrapped in double curly braces: `{{ ... }}`.
Inside the braces, you can write valid JavaScript code.

*   **Simple Variable**: `{{ $json.myField }}`
*   **Nested Field**: `{{ $json.address.city }}`
*   **Array Access**: `{{ $json.tags[0] }}`
*   **Math**: `{{ $json.price * $json.quantity }}`
*   **String Concatenation**: `{{ "Hello " + $json.firstName }}`
*   **Template Literals**: `{{ `Hello ${$json.firstName}` }}`

---

## Data Access Variables

These global objects give you access to the workflow's data context.

### 1. `$json` (Current Item)
Accesses the JSON payload of the *current item* being processed.
*   **Usage**: Most common variable.
*   **Example**: `{{ $json.email }}`

### 2. `$binary` (Binary Data)
Accesses metadata of binary files attached to the item.
*   **Usage**: Getting filenames, MIME types.
*   **Example**: `{{ $binary.data.fileName }}`

### 3. `$node` (Previous Nodes)
Accesses data from *any* previous node in the workflow.
*   **Syntax**: `$node["Node Name"].json.field`
*   **Context**: Automatically resolves to the "Paired Item" (see below).
*   **Example**: `{{ $node["Webhook"].json.body.id }}`

### 4. `$env` (Environment)
Accesses environment variables (Self-hosted only).
*   **Usage**: API keys, configuration flags.
*   **Example**: `{{ $env.MY_API_KEY }}`

### 5. `$vars` (Custom Variables)
Accesses variables defined in **Settings > Variables**.
*   **Usage**: Global constants.
*   **Example**: `{{ $vars.SLACK_CHANNEL_ID }}`

### 6. `$now` / `$today`
Luxon DateTime objects for the current time.
*   **Example**: `{{ $now.minus({ days: 1 }).toISO() }}`

---

## The "Paired Item" Concept

Understanding this is **critical** for loops and merging data.

When Node C references Node A (skipping Node B), n8n needs to know: "For the item I am processing right now in Node C, which item in Node A does it correspond to?"

*   **Automatic Resolution**: `{{ $node["Node A"].json.id }}`
    *   n8n automatically finds the ancestor item. This works 99% of the time.
*   **Manual Resolution**: `{{ $('Node A').item.json.id }}`
    *   Explicitly asks for the paired item.
*   **First Item**: `{{ $('Node A').first().json.id }}`
    *   Ignores pairing and always grabs the first item of Node A. Useful for "Global" config nodes that only output 1 item.
*   **All Items**: `{{ $('Node A').all() }}`
    *   Returns an array of all items. Useful for aggregation.

---

## Advanced Logic

Since expressions are JavaScript, you can do complex logic.

### Ternary Operator (If/Else)
```javascript
{{ $json.active ? "Active User" : "Inactive User" }}
```

### Short-circuit Evaluation
```javascript
// Use default if name is missing
{{ $json.name || "Anonymous" }}

// Safe access (Optional Chaining)
{{ $json.user?.profile?.email }}
```

### Array Operations
```javascript
// Join tags with a comma
{{ $json.tags.join(', ') }}

// Sum an array of numbers
{{ $json.prices.reduce((a, b) => a + b, 0) }}
```

### IIFE (Immediately Invoked Function Expression)
If you need multiple lines of logic or temporary variables, wrap it in a function.

```javascript
{{
  (() => {
    const firstName = $json.name.split(' ')[0];
    const domain = $json.email.split('@')[1];
    if (domain === 'gmail.com') {
      return `Personal: ${firstName}`;
    }
    return `Business: ${firstName}`;
  })()
}}
```

---

## Debugging Expressions

### 1. The Preview Pane
When you type an expression in the UI, the "Expression" tab shows the **Result** immediately based on the first item of input data.
*   **Tip**: If it says `[Object object]`, wrap it in `JSON.stringify(...)` to see the structure.

### 2. `[Object object]` Error
This happens when you try to put a JS Object into a field that expects a String.
*   **Fix**: Access a specific property (`$json.myObj.id`) or stringify it (`JSON.stringify($json.myObj)`).

### 3. "Referenced node is unexecuted"
You are trying to reference data from a node that hasn't run yet (or was skipped by an IF node).
*   **Fix**: Ensure the node is connected and has executed. Use optional chaining (`?.`) if the node might be skipped.

---

## Best Practices

1.  **Keep it Simple**: If your expression is more than 3 lines, consider using a **Code Node**. It's easier to read, debug, and version control.
2.  **Use `$json`**: Prefer `$json` over `$node["Previous Node"].json` whenever possible. It makes the node portable (you can copy-paste it without breaking references).
3.  **Type Safety**: Remember that input fields in n8n are typed.
    *   If a field expects a **Number**, your expression must return a Number (e.g., `{{ 123 }}`), not a String (`{{ "123" }}`).
    *   If a field expects a **Boolean**, return `true`/`false`.
4.  **Comments**: You can add comments inside expressions using standard JS syntax `//` or `/* */`.

```javascript
{{
  // Calculate tax
  $json.price * 0.2
}}
```
