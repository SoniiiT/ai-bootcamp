---
description: Comprehensive guide to data transformation, date handling (Luxon), and built-in functions in n8n.
applyTo: "**"
---

# Data Transformation & Manipulation Guide

n8n provides a rich set of tools for transforming data, ranging from simple one-line expressions to complex JavaScript logic.

## 1. Date & Time (Luxon)

n8n uses the [Luxon](https://moment.github.io/luxon/) library for all date and time operations. It is globally available in expressions and Code nodes.

### Built-in Variables
*   `$now`: Current timestamp as a Luxon `DateTime` object.
*   `$today`: Current timestamp rounded down to the start of the day (00:00:00).

### Common Operations

#### Formatting
Convert a date object to a string.
*   **ISO 8601**: `{{ $now.toISO() }}` -> `2023-10-27T14:30:00.000Z`
*   **Custom Format**: `{{ $now.toFormat('yyyy-MM-dd HH:mm') }}` -> `2023-10-27 14:30`
*   **Human Readable**: `{{ $now.toLocaleString() }}` -> `10/27/2023`

#### Parsing
Convert a string to a Luxon object.
*   **From ISO**: `{{ DateTime.fromISO('2023-10-27T14:30:00Z') }}`
*   **From SQL**: `{{ DateTime.fromSQL('2023-10-27 14:30:00') }}`
*   **From Custom Format**: `{{ DateTime.fromFormat('27/10/2023', 'dd/MM/yyyy') }}`
*   **From JS Date**: `{{ DateTime.fromJSDate(new Date()) }}`

#### Math (Add/Subtract)
*   **Add**: `{{ $now.plus({ days: 7, hours: 2 }) }}`
*   **Subtract**: `{{ $now.minus({ months: 1 }) }}`
*   **Start Of**: `{{ $now.startOf('month') }}` (First day of the month)
*   **End Of**: `{{ $now.endOf('week') }}` (Last moment of the week)

#### Timezones
*   **Set Zone**: `{{ $now.setZone('America/New_York') }}`
*   **Keep Local Time**: `{{ $now.setZone('America/New_York', { keepLocalTime: true }) }}`

#### Diff (Duration)
Calculate the difference between two dates.
```javascript
// Returns difference in days (e.g., 5.5)
{{ DateTime.fromISO($json.endDate).diff(DateTime.fromISO($json.startDate), 'days').days }}
```

---

## 2. String Manipulation

Standard JavaScript string methods are available, plus n8n helper methods.

### Built-in Helpers
*   `.camelCase()`: `{{ 'my string'.camelCase() }}` -> `myString`
*   `.snakeCase()`: `{{ 'my string'.snakeCase() }}` -> `my_string`
*   `.kebabCase()`: `{{ 'My String'.kebabCase() }}` -> `my-string`
*   `.startCase()`: `{{ 'myString'.startCase() }}` -> `My String`
*   `.slugify()`: `{{ 'My String!'.slugify() }}` -> `my-string`
*   `.extractUrl()`: Returns an array of URLs found in the text.

### Standard JS Methods
*   `.split(separator)`: `{{ 'a,b,c'.split(',') }}` -> `['a', 'b', 'c']`
*   `.replace(search, replacement)`: `{{ 'Hello World'.replace('World', 'n8n') }}`
*   `.trim()`: Removes whitespace from both ends.
*   `.toUpperCase()` / `.toLowerCase()`
*   `.includes(substring)`: Returns true/false.

---

## 3. Array & Object Manipulation

### Arrays
*   `.unique()`: Removes duplicate values from an array.
*   `.random()`: Returns a random element from the array.
*   `.join(separator)`: Joins array elements into a string.
*   `.map(callback)`: Transforms each element.
    *   `{{ $json.tags.map(tag => tag.name).join(', ') }}`
*   `.filter(callback)`: Filters elements.
    *   `{{ $json.items.filter(item => item.price > 100) }}`
*   `.sort(callback)`: Sorts the array.

### Objects
*   `Object.keys($json)`: Returns an array of keys.
*   `Object.values($json)`: Returns an array of values.
*   `JSON.stringify($json)`: Converts object to JSON string.
*   `JSON.parse(string)`: Converts JSON string to object.

---

## 4. JMESPath (JSON Querying)

n8n supports [JMESPath](https://jmespath.org/) for complex JSON querying and transformation without writing full JavaScript.

*   **Usage**: `.jmespath('query')` method on any object.
*   **Example**: Extract all emails from a list of users where age > 18.
    ```javascript
    {{ $json.users.jmespath("[?age > `18`].email") }}
    ```

### Common Patterns
*   **Projection**: `people[*].first_name` (Get list of all first names)
*   **Filter**: `people[?state == 'WA']` (Get people in WA)
*   **Pipe**: `people[*].first_name | sort(@)` (Get sorted names)

---

## 5. Cryptography & Hashing

In the **Code Node**, you can use the built-in Node.js `crypto` module.

```javascript
const crypto = require('crypto');

// MD5 Hash
const hash = crypto.createHash('md5').update('myString').digest('hex');

// SHA256 HMAC
const hmac = crypto.createHmac('sha256', 'secretKey').update('message').digest('hex');

// Random UUID
const uuid = crypto.randomUUID();
```

---

## 6. n8n Metadata Variables

Access system and workflow context.

| Variable | Description |
| :--- | :--- |
| `$execution.id` | Unique ID of the current execution. |
| `$execution.mode` | `production` (webhook/trigger) or `manual` (UI button). |
| `$workflow.id` | ID of the current workflow. |
| `$workflow.name` | Name of the current workflow. |
| `$env` | Access environment variables (Self-hosted only). |
| `$vars` | Access [Custom Variables](https://docs.n8n.io/variables/) defined in the UI. |

---

## 7. Accessing Other Nodes

You can reference data from *any* previous node, not just the immediate parent.

### Syntax
*   `$('Node Name')`: Returns a selector object for that node.

### Methods
*   `.item`: Returns the **paired item** (the item in the referenced node that corresponds to the current item being processed). This is the safest and most common method.
    *   `{{ $('Webhook').item.json.body.id }}`
*   `.first()`: Returns the very first item of the referenced node (index 0).
    *   `{{ $('Webhook').first().json.body.id }}`
*   `.last()`: Returns the last item.
*   `.all()`: Returns an array of *all* items from that node.
    *   `{{ $('Webhook').all()[0].json.body.id }}`

### The "Paired Item" Concept
n8n automatically tracks lineage. If you have 10 items flowing through your workflow, `$('Node A').item` knows exactly which of the 10 items in "Node A" generated the current item you are processing.

---

## 8. Best Practices

1.  **Use Luxon**: Always use Luxon for dates. Avoid native `Date` unless necessary for a specific library.
2.  **Fail Gracefully**: Use optional chaining (`?.`) in expressions to prevent errors if a field is missing.
    *   `{{ $json.user?.address?.city || 'Unknown' }}`
3.  **Complex Logic**: If an expression becomes hard to read (nested ternaries), move it to a **Code Node**.
4.  **Timezones**: Always be explicit about timezones. Servers usually run in UTC.
