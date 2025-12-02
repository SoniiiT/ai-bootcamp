# Rule 2 - Documentation Types and Structures

## Documentation Types

### 1. API Documentation
**Purpose:** Help developers understand and use APIs

**Structure:**
```markdown
# API Name

## Overview
Brief description of the API

## Authentication
How to authenticate?

## Endpoints

### GET /resource
Description

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|

**Response:**
```json
{ "example": "response" }
```

**Errors:**
| Code | Description |
|------|-------------|
```

**Best Practices:**
- All endpoints with examples
- Request/response examples as JSON
- Error codes with descriptions
- Authentication notes

---

### 2. User Manual
**Purpose:** Support end users in product usage

**Structure:**
```markdown
# Product Name - User Manual

## Introduction
What is the product? Who is it for?

## Getting Started
1. Installation/Registration
2. Initial configuration
3. First use

## Features

### Feature A
Step-by-step instructions

## Frequently Asked Questions (FAQ)

## Troubleshooting
```

**Best Practices:**
- Use simple language
- Screenshots where helpful
- Numbered steps
- FAQs for common issues

---

### 3. Reference Documentation
**Purpose:** Quick lookup of details

**Structure:**
```markdown
# Configuration Reference

## Overview
Where to configure? Format?

## Options

### option_name
- **Type:** string
- **Default:** "default"
- **Required:** No
- **Description:** What does this option do?
- **Example:** `option_name: "value"`
```

**Best Practices:**
- Alphabetically or thematically organized
- All options with type and default
- Examples for complex options
- Structure for searchability

---

### 4. Concept Documentation
**Purpose:** Understanding architecture and design

**Structure:**
```markdown
# Architecture Overview

## Introduction
Why this architecture?

## Components
Description of main components

## Data Flow
How does data flow through the system?

## Design Decisions
Why were certain decisions made?

## Diagrams
Architecture and sequence diagrams
```

**Best Practices:**
- Diagrams for visual overview
- Document decisions (ADRs)
- Consider target audience
- Update regularly

---

### 5. Runbook
**Purpose:** Operations guides for recurring tasks

**Structure:**
```markdown
# Runbook: [Task Name]

## Overview
- **Purpose:** What is achieved?
- **Frequency:** How often?
- **Duration:** Estimated time
- **Prerequisites:** What is needed?

## Procedure

### Step 1: [Title]
```bash
# Command
```
Expected result: ...

### Step 2: [Title]
...

## Verification
How to check success?

## Rollback
In case of problems: How to revert?

## Troubleshooting
Common problems and solutions
```

**Best Practices:**
- Each step individual and testable
- Commands copy-paste ready
- Rollback plan mandatory
- Test and update regularly

## Structure Principles

### Hierarchy
- Maximum 3 heading levels
- Logical grouping
- Consistent naming

### Navigation
- Table of contents for long documents
- Cross-references between related topics
- Breadcrumbs for deep structure

### Modularity
- One topic per document
- Reusable sections
- Link instead of duplicate
