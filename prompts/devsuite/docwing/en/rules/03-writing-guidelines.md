# Rule 3 - Writing Guidelines

## Language Principles

### Clarity
- **Simple language**: Short sentences, clear word choice
- **Active voice**: "Click on..." instead of "It is clicked on..."
- **Direct address**: "You can..." or imperative form "Execute..."
- **No filler words**: Avoid "actually", "somewhat", "so to speak"

### Consistency
- **Terminology**: Use chosen terms consistently throughout
- **Formatting**: Format similar elements the same way
- **Structure**: Build similar content similarly
- **Style**: Uniform tone throughout documentation

### Precision
- **Exact specifications**: Concrete numbers, paths, commands
- **Complete commands**: Copy-paste ready commands
- **Clear references**: Unambiguous references to other sections
- **Current information**: Version numbers, dates

## Audience-Appropriate Language

### For Developers
- Technical terms without explanation
- Code examples in relevant languages
- Concise explanations
- API details and implementation hints

**Example:**
```
The `getUserById` function performs an O(1) lookup in the cache
and falls back to the database on cache miss.
```

### For End Users
- Simple, non-technical language
- Step-by-step instructions
- Screenshots and visual aids
- Avoidance of jargon

**Example:**
```
Click the "Save" button to apply your changes.
A green checkmark indicates that saving was successful.
```

### For Administrators
- Technical details with context
- Security notes
- Configuration options complete
- Troubleshooting information

**Example:**
```
Set `max_connections` to a value between 100 and 500.
Values over 500 may impact performance.
Monitor active connections with `SHOW PROCESSLIST`.
```

## Formatting Rules

### Headings
```markdown
# Main Title (H1) - once per document
## Main Section (H2)
### Subsection (H3)
```

### Emphasis
- **Bold**: Important terms, UI elements
- *Italic*: Emphasis, new terms when introduced
- `Code`: Commands, filenames, code elements
- > Blockquotes: Important notes, warnings

### Lists
**Bullet points** for:
- Unordered collections
- Features or properties
- Options without sequence

**Numbering** for:
1. Step-by-step instructions
2. Prioritized lists
3. Sequential processes

### Tables
```markdown
| Column A | Column B | Column C |
|----------|----------|----------|
| Value 1  | Value 2  | Value 3  |
```

**Use for:**
- Parameter references
- Comparisons
- Structured data

### Code Blocks
````markdown
```language
// Code with syntax highlighting
```
````

**Always specify language:**
- `bash`, `powershell` for commands
- `json`, `yaml` for configuration
- `python`, `javascript`, etc. for code

### Note Boxes

**Info:**
> 💡 **Note:** Additional helpful information.

**Warning:**
> ⚠️ **Warning:** Important security note or limitation.

**Error:**
> ❌ **Caution:** Critical note that must be observed.

**Success:**
> ✅ **Tip:** Best practice or recommendation.

## Quality Criteria

### Checklist Before Publishing
- [ ] Spelling checked
- [ ] All links work
- [ ] Code examples tested
- [ ] Screenshots current
- [ ] Consistent terminology
- [ ] Target audience considered
- [ ] Structure logical
- [ ] Version information correct
