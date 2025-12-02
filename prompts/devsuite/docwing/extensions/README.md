# DocWing Extensions

## Overview

Extensions enhance DocWing with domain-specific or platform-specific documentation capabilities. They are **optional** and can be loaded in addition to DocWing Core as needed.

## Available Extensions

### ITGlue
IT documentation platform extension for MSPs.

**Contents:**
- Platform context (Organizations, Core Assets, Flexible Assets, Network Glue)
- Documentation standards for ITGlue
- Flexible Asset template guidelines
- Runbooks and SOP creation
- Integration and synchronization documentation

**Usage:**
```
# Load ITGlue extension
@extensions/itglue/de/context/platform.md
@extensions/itglue/de/rules/01-itglue-dokumentation.md
@extensions/itglue/de/rules/02-flexible-assets.md
@extensions/itglue/de/rules/03-runbooks-sops.md
@extensions/itglue/de/rules/04-integration-sync.md
```

---

---

## How to Use Extensions?

### Option 1: Mention When Needed
Simply tell DocWing that you want to use an extension:

```
"I want to document a Confluence space."
"Please use the API-Docs Extension."
```

DocWing will then automatically load the appropriate rules and contexts.

### Option 2: Manually Reference in Prompt
Add the extension files to your prompt context:

```
# Rules
@extensions/<extension-name>/en/rules/01-<rule-name>.md

# Context
@extensions/<extension-name>/en/context/platform.md
```

---

## Extension Structure

Each extension follows this folder structure:

```
extensions/
  <platform-name>/
    de/
      rules/
        01-<rule-name>.md
        02-<rule-name>.md
        ...
      context/
        platform.md
        ...
    en/
      rules/
        01-<rule-name>.md
        ...
      context/
        platform.md
        ...
```

---

## Creating Your Own Extension

You can create your own extensions for specific documentation domains:

### 1. Create Folder Structure
```
extensions/
  <your-platform>/
    de/
      rules/
      context/
    en/
      rules/
      context/
```

### 2. Define Rules
Create `.md` files in `rules/` that describe specific documentation requirements for your platform.

**Example extensions:**
- `confluence/` - Confluence-specific documentation patterns
- `mkdocs/` - MkDocs configuration and theming
- `openapi/` - Advanced OpenAPI/Swagger documentation
- `readme-io/` - ReadMe.io specific features

### 3. Add Context
Create `platform.md` in `context/` with background information about the platform.

### 4. Inform DocWing
Mention your extension in prompts or load the files directly.

---

## Extension Ideas

Potential extensions that could be created:

| Extension | Description |
|-----------|-------------|
| `confluence` | Confluence macros, spaces, page hierarchy |
| `mkdocs` | MkDocs plugins, themes, navigation |
| `docusaurus` | MDX components, versioning, i18n |
| `openapi` | Advanced API documentation patterns |
| `adr` | Architecture Decision Records |
| `changelog` | Keep a Changelog format |
| `readme-badges` | Shields.io badges and formatting |

---

## Best Practices for Extensions

✅ **Extend, don't replace:** Extensions should enhance DocWing Core, not replace it  
✅ **Clear boundaries:** Only platform-specific content in extensions  
✅ **Multilingual:** DE and EN support  
✅ **Documented:** Each extension should clearly explain its purpose  
✅ **Consistent:** Same structure as other extensions  
✅ **Examples:** Include concrete examples in rules

---

## Extension Template

Use this as a starting point for new extensions:

### `extensions/<name>/en/context/platform.md`
```markdown
# <Platform Name>

## Overview
Brief description of the platform.

## Key Concepts
- Concept 1
- Concept 2

## Resources
- [Official Documentation](url)
```

### `extensions/<name>/en/rules/01-basics.md`
```markdown
# Rule 1 - <Platform> Basics

## Purpose
What does this rule cover?

## Guidelines
...

## Examples
...
```
