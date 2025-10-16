# AI Bootcamp - Custom AI Models & Prompts

A comprehensive collection of custom AI models and prompt engineering templates designed for specialized use cases. This repository contains production-ready AI assistants with structured prompts, rules, and context for various professional scenarios.

## 🚀 Featured Models

### DevWing - Technical Copilot
**Your AI wingman for development and IT operations**

DevWing is a specialized technical assistant supporting developers and IT professionals with:
- Software development (Python, C#, JavaScript, TypeScript, Node.js)
- System administration (Windows Server, Active Directory, PowerShell)
- Container orchestration (Docker, Kubernetes)
- Cloud infrastructure (Azure, AWS)
- IT security and best practices

**Extension System:**  
DevWing features a modular extension architecture for platform-specific functionality:
- **Datto RMM Extension**: Full support for RMM component development (Monitors, Scripts, Applications)
- **Community Extensions**: Open for contributions (NinjaOne, Ansible, Terraform, etc.)

📂 **Location:** `prompts/devwing/`  
🌍 **Languages:** German (DE), English (EN)

---

### IHK Exam Preparation Models
Professional assistants for German Chamber of Commerce (IHK) IT specialist exams:

#### 1. **Projektantragshelfer** (Project Application Assistant)
Analyzes and evaluates IHK project applications against official criteria.

#### 2. **Projektdokumentationshelfer** (Project Documentation Helper)
Reviews project documentation with detailed evaluation matrices and improvement suggestions.

#### 3. **Projektgespraechshelfer** (Project Interview Preparation)
Prepares candidates for technical project interviews with realistic Q&A scenarios.

#### 4. **Projektpraesentationshelfer** (Project Presentation Assistant)
Optimizes project presentations with structure analysis and enhancement recommendations.

#### 5. **Pruefungsvorbereiter** (Exam Preparation Tool)
General exam preparation assistant with study plans and practice questions.

📂 **Location:** `prompts/projekt*/` and `prompts/pruefungsvorbereiter/`  
🌍 **Languages:** German (DE)

---

## 📁 Repository Structure

```
ai-bootcamp/
├── prompts/
│   ├── devwing/
│   │   ├── de/                    # German version
│   │   │   ├── system.md         # Core model definition
│   │   │   ├── rules/            # Behavioral rules
│   │   │   └── context/          # Background knowledge
│   │   ├── en/                    # English version
│   │   └── extensions/
│   │       ├── README.md         # Extension documentation
│   │       └── datto-rmm/        # Datto RMM extension
│   │           ├── de/
│   │           │   ├── rules/
│   │           │   └── context/
│   │           └── en/
│   │               ├── rules/
│   │               └── context/
│   ├── projektantragshelfer/      # Project application helper
│   ├── projektdokumentationshelfer/ # Documentation helper
│   ├── projektgespraechshelfer/   # Interview preparation
│   ├── projektpraesentationshelfer/ # Presentation assistant
│   └── pruefungsvorbereiter/      # Exam preparation
└── templates/
    ├── system.example.md          # Model definition template
    ├── rule.example.md            # Rule template
    └── context.example.md         # Context template
```

---

## 🎯 How to Use

### Option 1: Direct Integration
Copy the contents of `system.md`, `rules/*.md`, and `context/*.md` into your AI chat interface or custom GPT configuration.

### Option 2: File References (GitHub Copilot, Cursor, etc.)
Reference files directly in your prompts:
```
@prompts/devwing/en/system.md
@prompts/devwing/en/rules/01-response-style.md
@prompts/devwing/en/context/tech-stack.md
```

### Option 3: Use with Extensions
For platform-specific functionality (e.g., Datto RMM):
```
# Load DevWing Core
@prompts/devwing/en/system.md
@prompts/devwing/en/rules/*.md

# Add Datto RMM Extension
@prompts/devwing/extensions/datto-rmm/en/rules/*.md
@prompts/devwing/extensions/datto-rmm/en/context/platform.md
```

---

## 🛠️ Creating Your Own Model

Use the provided templates to create custom AI models:

1. **Copy template structure:**
   ```bash
   mkdir -p prompts/your-model/{de,en}/{rules,context}
   cp templates/system.example.md prompts/your-model/de/system.md
   ```

2. **Define your model:**
   - `system.md`: Core model description, role, and behavior
   - `rules/*.md`: Specific behavioral rules and guidelines
   - `context/*.md`: Domain knowledge and background information

3. **Add multilingual support:**
   - Create parallel DE and EN versions
   - Maintain consistent structure across languages

4. **Test and iterate:**
   - Test your model with various scenarios
   - Refine rules and context based on results

---

## 🌟 DevWing Extensions

### Creating Extensions

Extensions allow you to add platform-specific functionality without modifying core models.

**Structure:**
```
prompts/devwing/extensions/
└── your-platform/
    ├── de/
    │   ├── rules/
    │   └── context/
    └── en/
        ├── rules/
        └── context/
```

**See:** `prompts/devwing/extensions/README.md` for detailed guidelines.

### Available Extensions
- ✅ **Datto RMM** - Remote monitoring and management component development
- 🔄 **Community Contributions Welcome** - NinjaOne, Ansible, Terraform, etc.

---

## 🤝 Contributing

This is a **personal project**, but contributions are welcome, especially for:
- New extensions for RMM platforms (NinjaOne, Atera, Syncro, etc.)
- Translations and improvements to existing models
- Bug fixes and documentation enhancements

**How to contribute:**
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-extension`)
3. Commit your changes (`git commit -m 'feat: add amazing extension'`)
4. Push to the branch (`git push origin feature/amazing-extension`)
5. Open a Pull Request

---

## 📝 Model Structure Explained

### system.md
The core definition file containing:
- Model name and description
- Role and behavior guidelines
- Core competencies
- Interaction styles
- Quality standards
- Example interactions
- Model limitations

### rules/*.md
Behavioral rules that define:
- How the model should respond
- Technical scope and boundaries
- Interaction patterns
- Special requirements (e.g., scripting guidelines)

### context/*.md
Background knowledge including:
- Technology stack information
- Platform-specific documentation
- Domain knowledge
- Best practices

---

## 🌍 Language Support

All models support:
- **German (DE)**
- **English (EN)**

Directory structure clearly separates languages:
```
prompts/model-name/
├── de/  # German version
└── en/  # English version
```

---

## 📄 License

This project is open source. Feel free to use, modify, and distribute the prompts and models for your own purposes.

---

## 🙋 Support

For questions or issues:
- **GitHub Issues**: Report bugs or request features
- **Ask the AI**: Most models can explain their own usage
- **Extensions**: Contact respective extension authors

---

**Note:** This repository represents structured prompt engineering for production use. Models are designed for real-world applications and can be integrated into various AI platforms and tools.
