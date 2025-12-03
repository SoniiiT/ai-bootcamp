# Rule 1 - Response Style and Workflow Design

## Purpose
Ensures consistent, high-quality workflow designs and explanations across all platforms.

## When to Apply
- When designing new workflows.
- When explaining complex logic.
- When debugging existing flows.

## Core Requirements

### 1. Structure First
**Description:** Before providing specific code or configuration, outline the logical flow.
**Implementation:**
- Use a numbered list for steps.
- Clearly identify Trigger, Actions, and Logic.

**Example:**
> **Concept:**
> 1. **Trigger**: New Email arrives (Subject: "Invoice")
> 2. **Action**: Extract Attachment
> 3. **Logic**: If attachment is PDF -> Save to SharePoint
> 4. **Else**: Send Notification

### 2. Platform Specifics
**Description:** Adapt the output format to the requested platform.
**Guidelines:**
- **Missing Platform**: If no platform is mentioned, **ask explicitly** before generating a solution.
- **n8n**: Provide JSON or screenshot description.
- **Power Automate**: Describe steps ("Apply to each", "Compose") or Expressions.
- **Kestra**: Provide YAML code block.

### 3. Data Handling
**Description:** Be precise about data types.
**Must include:**
- JSON structure examples where relevant.
- Explicit mention of data type conversions (e.g., String to Integer).

## Output Format

### For Workflow Definitions
```yaml
# Example for Kestra / General YAML representation
id: my-flow
namespace: company.team
tasks:
  - id: step-1
    type: ...
```

### For Expressions
> Use `formatDateTime(utcNow(), 'yyyy-MM-dd')` to format the date.

## Common Mistakes
❌ **Mistake:** Providing a screenshot description without explaining the configuration inside the steps.
✅ **Instead:** "Add a 'Filter Array' action. Set 'From' to `outputs('Get_items')` and the condition to `item()?['Status']` equals `Active`."
