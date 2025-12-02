# Rule 1 - ITGlue Documentation Standards

## Documentation Structure in ITGlue

### Documentation Organization
Documentation in ITGlue follows a hierarchical structure:

```
Organization
├── Documents (Free-text documentation)
│   ├── Folders
│   │   └── Subfolders
│   └── Individual documents
├── Core Assets (Structured data)
│   ├── Configurations
│   ├── Contacts
│   ├── Passwords
│   ├── Locations
│   ├── Domains
│   └── SSL Certificates
└── Flexible Assets (Custom)
    ├── Applications
    ├── LAN
    └── [Custom Templates]
```

### Best Practices for Document Structure

**Folder Structure:**
```markdown
📁 Onboarding
📁 Standard Operating Procedures (SOPs)
📁 Network & Infrastructure
📁 Applications & Software
📁 Security & Compliance
📁 Emergency & Recovery
📁 Client-Specific
```

**Consistent Naming:**
- `[Client Name] - [Topic] - [Version/Date]`
- Example: `Acme Corp - VPN Setup - v2.0`

## Document Types in ITGlue

### 1. Free-Text Documents
**Use for:** Guides, SOPs, runbooks, process descriptions

**Structure:**
```markdown
# Document Title

## Overview
Brief description of the document

## Prerequisites
- Prerequisite 1
- Prerequisite 2

## Step-by-Step Guide
1. First step
2. Second step

## Troubleshooting
### Issue 1
Solution...

## Related Documentation
- [Link to Related Item]
```

### 2. Flexible Asset Documentation
**Use for:** Structured, repeatable information

**Example Application Template:**
```
Name: [Application Name]
Manufacturer: [Vendor]
Version: [Current Version]
License Type: [Subscription/Perpetual]
Server: [Tag to Configuration]
Responsible Contact: [Tag to Contact]
Documentation URL: [External Documentation]
Notes: [Free text]
```

### 3. Quick Notes
**Use for:** Quick, organization-related notes

**Format:**
```markdown
## [Date] - [Short Title]
[Note content]

Created by: [Username]
```

## Relationship Mapping

### Creating Links
Use **Related Items** to document relationships:

| From | To | Description |
|------|-----|-------------|
| Server (Configuration) | Application (Flexible Asset) | "Hosts this application" |
| Password | Configuration | "Access to this system" |
| Document | Configuration | "Documents this system" |
| Contact | Application | "Responsible for" |

### Best Practice Links
```markdown
Server → Passwords (Admin access)
Server → Applications (Hosted apps)
Server → Backup (Backup configuration)
Application → Contacts (Support/Responsible persons)
Network → Configurations (Devices in network)
```

## Password Documentation

### Embedded vs. General Passwords

**Use Embedded Passwords for:**
- Device-specific access (1:1 relationship)
- Local admin accounts
- Device-specific web interfaces

**Use General Passwords for:**
- Multi-use access
- Domain admin accounts
- SaaS applications
- Registrar/DNS access

### Password Structure
```markdown
Name: [Meaningful name]
Username: [Username]
Password: [Generated/Manual]
URL: [Access URL]
Category: [Choose appropriate]
Notes: [Additional information]
OTP: [If 2FA active]
```

## Versioning and Revisions

### Changelog in Document
```markdown
## Change History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 1.0 | 2024-01-15 | John Doe | Initial version |
| 1.1 | 2024-02-20 | Jane Smith | Updated Chapter 3 |
| 2.0 | 2024-03-10 | John Doe | Complete overhaul |
```

### Using ITGlue Revisions
- Revisions are automatically created on each save
- View previous versions via "Revisions" panel
- Restore to previous version when needed
