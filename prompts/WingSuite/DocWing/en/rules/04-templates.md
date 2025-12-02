# Rule 4 - Documentation Templates

## Purpose
This rule provides reusable templates for various documentation types.

## Templates

### README.md (Project)

```markdown
# Project Name

Short description of the project in 1-2 sentences.

## Features

- Feature 1
- Feature 2
- Feature 3

## Prerequisites

- Prerequisite 1
- Prerequisite 2

## Installation

```bash
# Installation command
```

## Usage

```bash
# Example command
```

## Configuration

Description of configuration options.

## Contributing

Notes for contributors.

## License

[License Type](LICENSE)
```

---

### API Endpoint

```markdown
## [HTTP Method] /path/to/endpoint

Brief description of the endpoint.

### Authentication

Describe required authentication.

### Parameters

#### Path Parameters

| Name | Type | Description |
|------|------|-------------|
| id   | string | Resource ID |

#### Query Parameters

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| limit | integer | No | 10 | Maximum count |

#### Request Body

```json
{
  "field": "value"
}
```

### Response

#### Success (200)

```json
{
  "id": "123",
  "name": "Example"
}
```

#### Errors

| Code | Description |
|------|-------------|
| 400  | Bad request |
| 404  | Not found |
| 500  | Server error |

### Example

```bash
curl -X GET "https://api.example.com/path" \
  -H "Authorization: Bearer TOKEN"
```
```

---

### Function/Method

```markdown
## functionName

Brief description of the function.

### Signature

```typescript
function functionName(param1: Type1, param2?: Type2): ReturnType
```

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| param1 | Type1 | Yes | Description |
| param2 | Type2 | No | Description |

### Returns

- **Type:** `ReturnType`
- **Description:** What is returned?

### Exceptions

| Exception | Condition |
|-----------|-----------|
| TypeError | When param1 is invalid |

### Examples

```typescript
// Simple example
const result = functionName("value");

// Complex example
const result = functionName("value", { option: true });
```

### See Also

- [Related Function](#)
```

---

### Configuration Option

```markdown
### optionName

Brief description of the option.

- **Type:** `string | number | boolean`
- **Default:** `"defaultvalue"`
- **Required:** No
- **Environment Variable:** `OPTION_NAME`

#### Description

Detailed description of the option and its effects.

#### Values

| Value | Description |
|-------|-------------|
| "a" | Meaning of A |
| "b" | Meaning of B |

#### Example

```yaml
config:
  optionName: "value"
```

#### Notes

> ⚠️ **Warning:** Special notes about usage.
```

---

### Changelog Entry

```markdown
## [Version] - YYYY-MM-DD

### Added
- New feature X (#123)
- Support for Y

### Changed
- Improved behavior of Z
- Updated dependencies

### Fixed
- Fix for issue A (#456)
- Correction in module B

### Removed
- Deprecated function C

### Security
- Fixed security vulnerability CVE-XXXX-XXXX

### Deprecated
- Function D will be removed in v2.0
```

---

### Troubleshooting Entry

```markdown
### Problem: [Brief Description]

#### Symptom

What does the user see? What error message appears?

```
Error message here
```

#### Cause

Why does this problem occur?

#### Solution

1. First step
2. Second step
3. Verification

```bash
# Solution command
```

#### Prevention

How to avoid this problem in the future?
```

## Usage Notes

1. Copy template and adapt
2. Replace placeholders with real content
3. Remove unnecessary sections
4. Maintain consistency with existing documentation
