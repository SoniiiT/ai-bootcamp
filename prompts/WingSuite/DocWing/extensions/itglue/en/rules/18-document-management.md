# Rule 18: Document & Asset Management

## Purpose
This rule defines standards for documenting document and asset management features in ITGlue.

## Document Creation

### Document Types
```markdown
## ITGlue Document Types

### Standard Documents
| Type | Usage |
|------|-------|
| Embedded | Created in ITGlue editor |
| Linked | External file (PDF, Word, etc.) |
| Template | Reusable template |

### Document Structure
| Element | Description |
|---------|-------------|
| Title | Descriptive name |
| Folder | Organizational assignment |
| Tags | Categorization |
| Content | Rich text content |
| Attachments | File attachments |
| Related Items | Linked assets |
```

### Document Template
```markdown
## Document: [Document Name]

### Metadata
| Property | Value |
|----------|-------|
| Organization | [Organization Name] |
| Folder | [Folder Path] |
| Created | [Date] |
| Author | [Name] |
| Last Modified | [Date] |
| Status | [Draft/Published/Archived] |

### Tags
[Tag1], [Tag2], [Tag3]

### Content
[Document content here]

### Related Items
- [Asset Type]: [Asset Name]
- [Asset Type]: [Asset Name]
```

## Sub-Documents

### Sub-Document Structure
```markdown
## Sub-Documents: Hierarchical Documentation

### Usage
Sub-documents enable nested document structures.

### Structure Example
```
📄 Main Document: Network Overview
├── 📄 Sub-Doc: Router Configuration
├── 📄 Sub-Doc: Switch Configuration
│   ├── 📄 Sub-Doc: VLAN Setup
│   └── 📄 Sub-Doc: Port Assignments
└── 📄 Sub-Doc: Firewall Rules
```

### Best Practices
| Rule | Description |
|------|-------------|
| Depth | Max 3 levels recommended |
| Naming | Clear, descriptive names |
| Inheritance | Permissions are inherited |
```

## Rich Text Editor

### Document Editor Features
```markdown
## ITGlue Rich Text Editor

### Formatting Options
| Feature | Shortcut | Description |
|---------|----------|-------------|
| Bold | Ctrl+B | Highlight text |
| Italic | Ctrl+I | Italicize text |
| Heading | - | H1, H2, H3 |
| List | - | Bullets/Numbering |
| Table | - | Table insertion |
| Code | - | Code block |
| Link | Ctrl+K | Hyperlink |

### Embedded Elements
| Element | Description |
|---------|-------------|
| Images | Drag & drop or upload |
| Screenshots | Direct paste |
| Diagrams | Link external images |
| Videos | Embedded links |
```

### @relate Function
```markdown
## @relate: Asset Linking in Text

### Usage
1. Type "@relate" in text
2. Select asset type
3. Search and select asset
4. Link is created

### Supported Types
- @relate Configuration
- @relate Password
- @relate Document
- @relate Contact
- @relate Flexible Asset

### Benefits
- Automatic linking
- Related Items are updated
- Bidirectional linking
```

## Revision Management

### Revision Documentation
```markdown
## Document Revisions

### Automatic Versioning
| Trigger | Description |
|---------|-------------|
| Save | New version on each change |
| Time-based | Consolidation after inactivity |

### Version View
| Column | Description |
|--------|-------------|
| Version | Version number |
| Date | Change timestamp |
| Author | Editor |
| Changes | Diff view |

### Restore
1. Open "History"
2. Select version
3. Click "Restore"
4. New version based on old is created
```

### Comparison Function
```markdown
## Compare Versions

### Diff View
| Display | Meaning |
|---------|---------|
| Green | Added |
| Red | Removed |
| Yellow | Changed |

### Perform Comparison
1. Open History
2. Select two versions
3. Click "Compare"
4. Analyze differences
```

## Audit Trail

### Audit Documentation
```markdown
## Document Audit Trail

### Logged Actions
| Action | Details |
|--------|---------|
| Created | Creation with author |
| Updated | Change with user |
| Viewed | Access (optional) |
| Deleted | Deletion with user |
| Restored | Restoration |
| Permission Changed | Permission change |

### Audit Report
| Column | Description |
|--------|-------------|
| Timestamp | Time of action |
| User | Performing user |
| Action | Type of action |
| Details | Additional information |

### Retention
- Audit logs stored for [X] days
- Export for compliance available
```

## Asset Management

### Managing Configurations
```markdown
## Configuration Management

### Configuration Fields
| Field | Description | Required |
|-------|-------------|----------|
| Name | Asset name | ✅ |
| Configuration Type | Server, Workstation, etc. | ✅ |
| Configuration Status | Active, Inactive, etc. | ✅ |
| Organization | Associated org | ✅ |
| Location | Location | Optional |
| Serial Number | Serial number | Optional |
| Notes | Additional info | Optional |

### Configuration Types
- Server
- Workstation
- Network Device
- Mobile Device
- Virtual Machine
- Printer
- [Custom Types]
```

### Asset Lifecycle
```markdown
## Asset Lifecycle Management

### Status Workflow
| Status | Description |
|--------|-------------|
| Active | In production |
| Inactive | Not in use |
| Decommissioned | Out of service |
| In Repair | Being repaired |
| Pending | Awaiting setup |

### Lifecycle Documentation
| Phase | Action | Documentation |
|-------|--------|---------------|
| Procurement | Create asset | Basic info |
| Deployment | Put into operation | Config details |
| Operations | Ongoing use | Updates, changes |
| Maintenance | Maintenance | Maintenance logs |
| Retirement | Decommissioning | Archiving |
```

## Flexible Assets

### Document Flexible Asset
```markdown
## Flexible Asset: [Asset Type Name]

### Type Definition
| Property | Value |
|----------|-------|
| Name | [Asset Type Name] |
| Icon | [Icon Name] |
| Description | [Description] |
| Tracking | Enabled/Disabled |

### Fields
| Field Name | Type | Required | Description |
|------------|------|----------|-------------|
| [Field 1] | Text | ✅ | [Description] |
| [Field 2] | Number | ❌ | [Description] |
| [Field 3] | Select | ✅ | [Options] |
| [Field 4] | Checkbox | ❌ | [Description] |
| [Field 5] | Date | ❌ | [Description] |
| [Field 6] | Text Area | ❌ | [Description] |
| [Field 7] | Upload | ❌ | [Description] |
| [Field 8] | Password | ❌ | Embedded Password |
| [Field 9] | Tag | ❌ | [Tag Options] |

### Relationships
- Organizations: [Yes/No]
- Configurations: [Yes/No]
- Contacts: [Yes/No]
- Other Flexible Assets: [List]
```

## Password Management (Assets)

### Password Categories
```markdown
## Password Categories

### Standard Categories
| Category | Usage |
|----------|-------|
| Admin | Administrator access |
| User | User access |
| Service | Service accounts |
| API | API keys |
| WiFi | Wireless passwords |
| Encryption | Encryption keys |

### Category-Based Permissions
| Category | IT Team | Management | Customers |
|----------|---------|------------|-----------|
| Admin | ✅ | ❌ | ❌ |
| User | ✅ | ✅ | ✅ |
| Service | ✅ | ❌ | ❌ |
| API | ✅ | ❌ | ❌ |
```

### Embedded Passwords
```markdown
## Embedded Passwords

### Description
Embed passwords directly in documents or Flexible Assets.

### Usage
1. Insert "Embedded Password" in editor
2. Enter password details
3. Choose position in document
4. Save

### Security
- Embedded Passwords inherit document permissions
- Separate audit logs
- Can be exported separately
```

## Contact Management

### Document Contacts
```markdown
## Contact Management

### Contact Fields
| Field | Description |
|-------|-------------|
| Name | Full name |
| Title | Title/Position |
| Email | Email address |
| Phone | Phone number |
| Contact Type | IT, Management, etc. |
| Location | Assigned location |
| Organization | Associated org |

### Contact Types
| Type | Description |
|------|-------------|
| Primary Contact | Main contact |
| Technical Contact | Technical contact |
| Billing Contact | Billing contact |
| Emergency Contact | Emergency contact |
```

## Best Practices

### Asset Management Best Practices
```markdown
## Best Practices: Asset Management

### Documentation
- Maintain complete base data
- Regular updates
- Link Related Items
- Use tags consistently

### Lifecycle
- Keep status current
- Document decommissioning
- Archive instead of delete

### Quality
- Use templates
- Follow naming conventions
- Establish review process
```

## Document Management Checklist

### Creation
- [ ] Document type selected
- [ ] Folder assigned
- [ ] Tags added
- [ ] Related Items linked

### Quality
- [ ] Content complete
- [ ] Formatting correct
- [ ] Images/screenshots inserted
- [ ] @relate links added

### Lifecycle
- [ ] Creation date documented
- [ ] Review date scheduled
- [ ] Responsible party named

### Security
- [ ] Permissions verified
- [ ] Embedded Passwords secured
- [ ] Audit trail active
