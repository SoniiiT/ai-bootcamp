# Rule 16: Content Management

## Purpose
This rule defines standards for documenting content management features in ITGlue, including bulk operations, GlueConnect, and checklists.

## Bulk Operations

### Document Bulk Operations
```markdown
## ITGlue Bulk Operations

### Available Bulk Actions
| Action | Description | Asset Types |
|--------|-------------|-------------|
| Bulk Edit | Edit multiple entries at once | All |
| Bulk Delete | Delete multiple entries | All |
| Bulk Move | Move to another organization | Configs, Docs |
| Bulk Copy | Copy to another organization | Docs |
| Bulk Tag | Add/remove tags | All |
| Bulk Archive | Archive | All |

### Procedure
1. Navigate to asset list
2. Select entries via checkbox
3. Open "Bulk Actions" menu
4. Select action
5. Confirm
```

### Bulk Edit Documentation
```markdown
## Bulk Edit: Mass Editing

### Supported Fields
| Asset Type | Editable Fields |
|------------|-----------------|
| Configurations | Status, Type, Tags, Location |
| Contacts | Type, Tags, Location |
| Passwords | Category, Tags |
| Documents | Folder, Tags |

### Limitations
- Maximum per bulk: [Limit]
- Not all fields bulk-editable
- Flexible Asset Fields: Limited
```

## GlueConnect

### GlueConnect Documentation
```markdown
## GlueConnect: Document Linking

### Description
GlueConnect enables linking documents across organizations.

### Use Cases
| Scenario | Description |
|----------|-------------|
| MSP Internal Docs | Share documentation for all customers |
| Templates | Distribute template documents |
| SOPs | Share standard processes |
| Compliance | Policy documents |

### Configuration
1. Create document in "Global" organization
2. Enable GlueConnect for document
3. Select target organizations
4. Start synchronization
```

### GlueConnect Template
```markdown
## GlueConnect Document: [Document Name]

### Metadata
| Property | Value |
|----------|-------|
| Source Organization | Global / [Name] |
| Linked Orgs | [Count] |
| Last Sync | [Date] |
| Status | Active/Inactive |

### Linked Organizations
| Organization | Status | Last Sync |
|--------------|--------|-----------|
| [Org 1] | Synchronized | [Date] |
| [Org 2] | Synchronized | [Date] |

### Change History
| Date | Change | Author |
|------|--------|--------|
| [Date] | [Description] | [Name] |
```

## Checklists

### Checklists in Documents
```markdown
## ITGlue Checklists

### Checklist Types
| Type | Usage |
|------|-------|
| Onboarding | New customer setup |
| Offboarding | Customer exit |
| Server Setup | Server deployment |
| Security Review | Security assessment |
| Maintenance | Maintenance work |

### Checklist Format in Documents
```markdown
## Checklist: [Name]

### Phase 1: [Phase Name]
- [ ] Task 1
- [ ] Task 2
  - [ ] Subtask 2.1
  - [ ] Subtask 2.2
- [ ] Task 3

### Phase 2: [Phase Name]
- [ ] Task 4
- [ ] Task 5
```

### Checklist Tracking
| Property | Value |
|----------|-------|
| Progress | X/Y completed |
| Last Updated | [Date] |
| Editor | [Name] |
```

## Document Folders

### Document Folder Structure
```markdown
## Document Folders: [Organization Name]

### Folder Structure
```
📁 Root
├── 📁 General
│   ├── 📁 Contacts
│   └── 📁 Contracts
├── 📁 Technical
│   ├── 📁 Network
│   ├── 📁 Servers
│   └── 📁 Workstations
├── 📁 Processes
│   ├── 📁 SOPs
│   └── 📁 Checklists
└── 📁 Archive
```

### Folder Guidelines
| Rule | Description |
|------|-------------|
| Naming | [Convention] |
| Max Depth | 3 levels recommended |
| Archiving | After [Time Period] |
```

### Folder Best Practices
```markdown
## Best Practices: Folder Structure

### Recommendations
- Consistent structure across all orgs
- Don't nest too deep
- Descriptive names
- Archive folder for old documents

### Standard Structure (MSP)
| Folder | Content |
|--------|---------|
| /General | Overview documents |
| /Network | Network documentation |
| /Servers | Server documentation |
| /Workstations | Client documentation |
| /Applications | Applications |
| /Procedures | SOPs and processes |
| /Archive | Archived documents |
```

## Document Copy & Move

### Copy/Move Documentation
```markdown
## Copying and Moving Documents

### Copy
| Property | Description |
|----------|-------------|
| Action | Creates copy at destination |
| Original | Remains intact |
| Links | Not copied |
| Embedded Passwords | Optional copy |

### Move
| Property | Description |
|----------|-------------|
| Action | Moves to destination |
| Original | Removed |
| Links | Dissolved |
| Embedded Passwords | Moved |

### Procedure
1. Select document
2. Actions > Copy/Move
3. Choose target organization
4. Choose target folder
5. Confirm
```

## Tags

### Tag Management Documentation
```markdown
## ITGlue Tags: Overview

### Tag Categories
| Category | Tags | Usage |
|----------|------|-------|
| Status | Active, Inactive, Pending | Asset status |
| Priority | Critical, High, Normal, Low | Importance |
| Type | Server, Workstation, Network | Asset type |
| Location | HQ, Branch, Remote | Location |
| Compliance | PCI, HIPAA, SOC2 | Compliance |

### Tag Guidelines
- Consistent naming (CamelCase)
- No duplicates
- Regular cleanup
- Document all tags
```

### Using Tag Filters
```markdown
## Tag-Based Filtering

### Filter Examples
| Filter | Result |
|--------|--------|
| tag:Critical | All critical assets |
| tag:Server AND tag:Production | Production servers |
| -tag:Archived | Non-archived |

### Dashboards with Tags
- Create dashboard by tags
- Display tag statistics
- Quick access to tag-filtered lists
```

## Document Revisions

### Revision Documentation
```markdown
## Document Revisions

### Revision History
| Version | Date | Author | Change |
|---------|------|--------|--------|
| v3 | [Date] | [Name] | [Description] |
| v2 | [Date] | [Name] | [Description] |
| v1 | [Date] | [Name] | Initial version |

### View Revision
1. Open document
2. Click "History"
3. Select version
4. Compare or restore

### Restoration
- Any version can be restored
- Creates new version based on old
- Changes are logged
```

## Archiving

### Archiving Guidelines
```markdown
## Archiving: Guidelines

### When to Archive
| Situation | Action |
|-----------|--------|
| Project completed | Archive documents |
| Customer inactive | Archive all assets |
| Outdated information | Archive document |
| After retention | Auto-archive |

### Archiving Process
1. Select asset
2. Set status to "Archived"
3. Move to archive folder
4. Add archiving tag

### Finding Archived Content
- Filter: Status = Archived
- Search in archive folders
- Enable Include Archived in search
```

## Content Lifecycle

### Lifecycle Documentation
```markdown
## Content Lifecycle Management

### Lifecycle Phases
| Phase | Status | Action |
|-------|--------|--------|
| Draft | Draft | In progress |
| Review | Needs Review | Review required |
| Active | Published | Live and current |
| Outdated | Outdated | Update needed |
| Archived | Archived | No longer active |

### Automatic Triggers
| Age | Action |
|-----|--------|
| 90 days | Review reminder |
| 180 days | Outdated flag |
| 365 days | Archiving suggestion |

### Review Cycle
- Quarterly review of all critical docs
- Annual review of all documents
- Automatic notifications
```

## Content Management Best Practices

### Best Practices
```markdown
## Content Management: Best Practices

### Organization
- Consistent folder structure
- Uniform naming conventions
- Use tags consistently
- Regular cleanup tasks

### Quality
- Use templates
- Follow review process
- Archive outdated content
- Keep links current

### Collaboration
- GlueConnect for shared docs
- Define responsibilities
- Document changes
- Establish feedback process
```

## Content Management Checklist

### Folder Structure
- [ ] Standard structure defined
- [ ] Applied to all orgs
- [ ] Documented

### Tags
- [ ] Tag catalog created
- [ ] Guidelines documented
- [ ] Team trained

### Lifecycle
- [ ] Review cycles defined
- [ ] Archiving guidelines
- [ ] Automation configured

### GlueConnect
- [ ] Shared docs identified
- [ ] GlueConnect configured
- [ ] Sync status monitored
