# n8n Code Node Guide

## Overview
The Code node is the ultimate escape hatch in n8n. It allows you to execute custom JavaScript (Node.js) or Python logic when built-in nodes cannot handle a specific transformation or logic requirement.

## 1. Execution Modes

### Run Once for All Items (Recommended)
*   **Behavior**: The code executes **exactly once**, regardless of how many items enter the node.
*   **Input**: You receive an array of all items (`items` or `$input.all()`).
*   **Output**: You must return an array of objects.
*   **Use Cases**:
    *   Aggregation (Sum, Average).
    *   Sorting / Filtering based on the whole set.
    *   Pivoting data (Array to Object).
    *   Batch processing.

### Run Once for Each Item
*   **Behavior**: The code executes **N times**, where N is the number of input items.
*   **Input**: You receive a single item (`item` or `$input.item`).
*   **Output**: You return a single object (or `undefined` to filter it out).
*   **Use Cases**:
    *   Simple 1:1 transformations (e.g., Regex on a string).
    *   Calculations per row.
*   **Performance Warning**: Slower for large datasets (1000+ items) because of the overhead of invoking the sandbox N times.

## 2. JavaScript (Node.js)

### Data Structure
n8n uses a specific structure. Every item is an object with two keys:
1.  `json`: The actual data (Object).
2.  `binary`: Binary file data (Object, optional).

**Correct Return Format:**
```javascript
return [
  {
    json: {
      id: 1,
      name: "Alice"
    },
    binary: {
      image: {
        data: "base64string...",
        mimeType: "image/png",
        fileName: "alice.png"
      }
    }
  }
];
```

### Built-in Libraries & Globals
*   `$input`: Access input data.
    *   `$input.all()`: All items.
    *   `$input.first()`: First item.
    *   `$input.last()`: Last item.
    *   `$input.item`: Current item (in "Run for Each" mode).
*   `$node`: Access node metadata.
*   `$env`: Access environment variables (Self-hosted only).
*   `DateTime`: [Luxon](https://moment.github.io/luxon/) library for date manipulation.
*   `require()`:
    *   **Cloud**: Limited to `crypto`, `lodash`, `moment`.
    *   **Self-hosted**: Can import any npm package if `NODE_FUNCTION_ALLOW_EXTERNAL` is set in `.env`.

### Common Snippets

#### Merging Data from Two Previous Nodes manually
```javascript
const users = $("Get Users").all();
const orders = $("Get Orders").all();

// Join on userId
return orders.map(order => {
  const user = users.find(u => u.json.id === order.json.userId);
  return {
    json: {
      ...order.json,
      userName: user ? user.json.name : "Unknown"
    }
  };
});
```

#### Flattening Nested Arrays
If an API returns `{ "users": [...] }` and you want one item per user:
```javascript
// Input: [{ json: { users: [ {id:1}, {id:2} ] } }]
const users = items[0].json.users;

return users.map(user => {
  return {
    json: user
  };
});
```

#### Handling Binary Data
Creating a text file from a string:
```javascript
const myString = "Hello World";
const buffer = Buffer.from(myString, 'utf8');

return [
  {
    json: { success: true },
    binary: {
      data: {
        data: buffer.toString('base64'),
        mimeType: 'text/plain',
        fileName: 'hello.txt'
      }
    }
  }
];
```

## 3. Python (Beta)

### Configuration
*   **Python Version**: Depends on the environment (usually Python 3).
*   **Libraries**: Standard library is available. External `pip` packages must be installed in the n8n container/environment.

### Syntax Differences
*   **Input**: `_input.all()` returns a list of dictionaries.
*   **Output**: Return a list of dictionaries. n8n automatically wraps them in the `json` key structure.

```python
# Python Example
items = _input.all()
output = []

for item in items:
    # Access JSON data directly
    data = item.json
    
    # Logic
    data['new_field'] = data['value'] * 2
    
    output.append(item)

return output
```

## 4. Debugging Code Nodes
*   **console.log()**: Output is shown in the **Browser Console** (F12), NOT in the n8n UI output.
*   **Browser Console**: Open DevTools -> Console to see your logs.
*   **Throw Error**: `throw new Error("My custom error")` will stop the workflow and show the message in the UI.

## 5. Best Practices
1.  **Always Return Arrays**: Even if you output a single item, wrap it in `[]`.
2.  **Preserve Binary**: If you map over items, make sure you don't lose the `binary` key if you need it later.
    *   *Bad*: `return items.map(i => ({ json: { ...i.json, new: 1 } }))` (Loses binary)
    *   *Good*: `return items.map(i => ({ ...i, json: { ...i.json, new: 1 } }))` (Keeps binary)
3.  **Memory Usage**: Avoid creating massive arrays in memory if processing millions of rows. Use `Split In Batches` before the Code node if necessary.
