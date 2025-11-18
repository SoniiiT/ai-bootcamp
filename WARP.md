# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository Overview

This is the **AI Bootcamp** repository - a collection of custom AI models and prompt engineering templates for specialized use cases. The repository contains production-ready AI assistants with structured prompts, rules, and context for various professional scenarios.

**Key Models:**
- **DevWing**: Technical copilot for developers and IT professionals (supports German and English)
- **IHK Exam Preparation Models**: German-language assistants for IHK IT specialist exams (Projektantragshelfer, Projektdokumentationshelfer, Projektgespraechshelfer, Projektpraesentationshelfer, Pruefungsvorbereiter)

## Repository Structure

```
ai-bootcamp/
├── prompts/                    # All AI model definitions
│   ├── devwing/               # DevWing model (DE + EN)
│   │   ├── de/               # German version
│   │   │   ├── system.md     # Core model definition
│   │   │   ├── rules/        # Behavioral rules (numbered)
│   │   │   └── context/      # Background knowledge
│   │   ├── en/               # English version
│   │   │   ├── system.md
│   │   │   ├── rules/
│   │   │   └── context/
│   │   └── extensions/        # Modular extensions
│   │       ├── README.md
│   │       └── datto-rmm/    # Platform-specific extension
│   ├── projektantragshelfer/  # IHK models (German only)
│   ├── projektdokumentationshelfer/
│   ├── projektgespraechshelfer/
│   ├── projektpraesentationshelfer/
│   └── pruefungsvorbereiter/
├── templates/                  # Templates for creating new models
│   ├── system.example.md      # Model definition template
│   ├── rule.example.md        # Rule template
│   └── context.example.md     # Context template
└── .ai/                       # AI IDE instructions
    └── ai-ide-instructions.md # Prompt builder assistant
```

## Architecture Patterns

### Three-Tier Model Structure

Each AI model follows a consistent three-tier architecture:

1. **system.md** - Core model definition containing:
   - Model name, description, and purpose
   - Role and behavior guidelines
   - Core competencies
   - Interaction styles and response structure
   - Quality standards
   - Example interactions
   - Model limitations

2. **rules/** - Behavioral rules (numbered files):
   - `01-response-style.md` - Communication patterns
   - `02-technical-scope.md` - Domain boundaries
   - `03-interaction.md` - Interaction patterns
   - `04-general-script.md` - Scripting guidelines (if applicable)
   - Additional platform-specific rules in extensions

3. **context/** - Background knowledge:
   - `tech-stack.md` - Technology information
   - `platform.md` - Platform-specific documentation (in extensions)
   - Domain knowledge and best practices

### Extension System (DevWing)

DevWing uses a modular extension architecture:
- **Core model** defines base functionality
- **Extensions** add platform-specific capabilities without modifying core
- Extensions follow same structure: `rules/` and `context/` folders
- Currently available: Datto RMM extension (DE + EN)

### Multilingual Support

Models support multiple languages with parallel directory structures:
- `/de/` for German versions
- `/en/` for English versions
- Maintain consistent structure across languages
- IHK models are German-only (DE only)

## Creating New Models

Use the templates in `templates/` as starting points:

1. **Create folder structure:**
   ```powershell
   New-Item -ItemType Directory -Force -Path "prompts\your-model\de\rules"
   New-Item -ItemType Directory -Force -Path "prompts\your-model\de\context"
   New-Item -ItemType Directory -Force -Path "prompts\your-model\en\rules"
   New-Item -ItemType Directory -Force -Path "prompts\your-model\en\context"
   Copy-Item "templates\system.example.md" -Destination "prompts\your-model\de\system.md"
   Copy-Item "templates\system.example.md" -Destination "prompts\your-model\en\system.md"
   ```

2. **Define model components:**
   - Edit `system.md` with core description and behavior
   - Create numbered rule files in `rules/` (e.g., `01-response-style.md`)
   - Add domain knowledge in `context/` files

3. **Follow naming conventions:**
   - Use lowercase with hyphens for folder names (e.g., `project-helper`)
   - Number rule files sequentially: `01-`, `02-`, `03-`
   - Use descriptive names for context files

## Creating DevWing Extensions

To add platform-specific functionality to DevWing:

1. **Create extension structure:**
   ```powershell
   New-Item -ItemType Directory -Force -Path "prompts\devwing\extensions\your-platform\de\rules"
   New-Item -ItemType Directory -Force -Path "prompts\devwing\extensions\your-platform\de\context"
   New-Item -ItemType Directory -Force -Path "prompts\devwing\extensions\your-platform\en\rules"
   New-Item -ItemType Directory -Force -Path "prompts\devwing\extensions\your-platform\en\context"
   ```

2. **Add platform-specific content:**
   - Create numbered rule files for platform requirements
   - Add `platform.md` in `context/` with platform documentation
   - Maintain both DE and EN versions

3. **Document the extension:**
   - Update `prompts/devwing/extensions/README.md`
   - Add usage examples and when to use the extension

See `prompts/devwing/extensions/README.md` for detailed extension guidelines.

## Key Principles

### For Model Development
- **Consistency**: Follow template structure for all models
- **Clarity**: Clear separation between rules, context, and system definition
- **Modularity**: Use extensions for platform-specific features (DevWing)
- **Multilingual**: Maintain parallel DE/EN structures where applicable
- **Documentation**: Each model should explain its own usage and limitations

### For Prompt Engineering
- Models should be production-ready and immediately usable
- Include example interactions demonstrating expected behavior
- Define clear boundaries and limitations
- Specify response structure for different query types
- Include quality standards and best practices

### For Code/Scripts in Models
- Production-ready code with error handling
- Comments only for important aspects (focus on "why" not "what")
- Security considerations explicitly noted
- Examples should be immediately usable

## File Naming Conventions

- **Models**: `lowercase-with-hyphens` (e.g., `projektantragshelfer`, `devwing`)
- **System files**: `system.md` (core definition), `system-compact.md` (optional shorter version)
- **Rule files**: `01-descriptive-name.md`, `02-descriptive-name.md` (numbered sequentially)
- **Context files**: `descriptive-name.md` (e.g., `tech-stack.md`, `platform.md`)
- **Templates**: `*.example.md`

## Integration Patterns

Models from this repository can be integrated into AI tools in several ways:

1. **Direct Integration**: Copy contents of system.md, rules, and context into AI chat interface
2. **File References**: Use `@prompts/model-name/lang/system.md` syntax (GitHub Copilot, Cursor)
3. **Extension Loading**: Load core model + extensions as needed (for DevWing)

## Quality Standards

When working with this repository:
- Maintain existing structure and patterns
- Keep templates up-to-date with any structural changes
- Ensure bilingual support for DevWing (DE + EN)
- Test models with various scenarios before considering them complete
- Document any new conventions or patterns in this file
