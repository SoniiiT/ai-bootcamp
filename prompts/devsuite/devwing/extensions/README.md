# DevWing Extensions

## Overview

Extensions enhance DevWing with platform-specific functionality. They are **optional** and can be loaded in addition to DevWing Core as needed.

## Available Extensions

### Datto RMM Extension
**Folder:** `extensions/datto-rmm/`

**Description:**  
Extends DevWing with full support for **Datto RMM** (Remote Monitoring and Management) component development.

**Includes:**
- Monitor components with Result/Diagnostic blocks
- Datto RMM Input Variables (String, Selection, Boolean, Date)
- Agent Variables (CS_*, UDF_*)
- Post-Conditions and Warning handling
- Naming conventions for Datto Components
- Platform-specific best practices

**When to use:**  
When you want to develop Datto RMM Components (Monitors, Scripts, Applications).

---

## How to Use Extensions?

### Option 1: Mention When Needed
Simply tell DevWing that you want to use an extension:

```
"I want to create a Datto RMM Monitor."
"Please use the Datto RMM Extension."
```

DevWing will then automatically load the appropriate rules and contexts.

### Option 2: Manually Reference in Prompt
Add the extension files to your prompt context:

**For Datto RMM:**
```
# Rules
@extensions/datto-rmm/en/rules/01-monitor.md
@extensions/datto-rmm/en/rules/02-general-script.md
@extensions/datto-rmm/en/rules/03-naming.md

# Context
@extensions/datto-rmm/en/context/platform.md
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

You can create your own extensions for other platforms:

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
Create `.md` files in `rules/` that describe specific requirements of your platform.

**Example:** `01-component-structure.md`

### 3. Add Context
Create `platform.md` in `context/` with background information about the platform.

### 4. Inform DevWing
Mention your extension in prompts or load the files directly.

---

## Best Practices for Extensions

✅ **Extend, don't replace:** Extensions should enhance DevWing Core, not replace it  
✅ **Clear boundaries:** Only platform-specific content in extensions  
✅ **Multilingual:** DE and EN support  
✅ **Documented:** Each extension should clearly explain its purpose  
✅ **Consistent:** Same structure as other extensions

---

---

## About This Project

DevWing is a **personal project** created to support my daily work as a developer and IT professional. Since I personally use **Datto RMM** in my work environment, the Datto RMM Extension is the only RMM platform extension I can develop and maintain based on real-world experience.

### Community Contributions Welcome

If you use **other RMM platforms** (NinjaOne, Atera, Syncro, etc.) and would like to create extensions for them:

🤝 **Contributions are welcome!**  
- Fork the repository
- Create your own extension following the structure shown above
- Submit a pull request
- Share your extension with the community

Since I don't have access to other RMM platforms, I cannot develop or maintain extensions for them. Community contributions would help expand DevWing's capabilities for more platforms.

---

## Support

For questions about extensions:
- **Ask DevWing itself:** "How do I use the Datto RMM Extension?"
- **Community Extensions:** Reach out to the respective extension author
