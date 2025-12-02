# Rule 12: Cooper Copilot Features

## Purpose
This rule defines standards for documenting Cooper Copilot AI features in ITGlue.

## Cooper Copilot Overview

### Feature Documentation
```markdown
## Cooper Copilot: Configuration Overview

### Activation Status
| Feature | Status |
|---------|--------|
| Cooper Copilot | Enabled/Disabled |
| Smart Relate | Enabled/Disabled |
| Smart SOP Generator | Enabled/Disabled |
| Smart Assist | Enabled/Disabled |

### Configuration
- Admin > Settings > Cooper Copilot
- Toggle for enable/disable
```

## Smart SOP Generator

### SOP Generator Documentation
```markdown
## Cooper Copilot Smart SOP Generator

### Description
Automatic creation of Standard Operating Procedures (SOPs) by recording browser activities.

### Prerequisites
- ITGlue Chrome Extension installed
- Cooper Copilot enabled
- User logged into ITGlue

### Supported Actions
- Browser navigation
- Element clicks
- Text input
- Datto RMM Web Remote Sessions
- Manual screenshots

### Workflow
1. Open Extension
2. Start SOP Generator
3. Perform actions
4. Review preview
5. Hide sensitive information
6. Save document to ITGlue
```

### SOP Documentation Template
```markdown
## SOP: [Process Name]

### Overview
| Property | Value |
|----------|-------|
| Created On | [Date] |
| Created With | Cooper Copilot SOP Generator |
| Target Audience | [User Group] |
| Organization | [Organization] |

### Steps
[Automatically generated steps with screenshots]

### Notes
- Sensitive data has been hidden
- Screenshots have been edited (if applicable)
```

## Smart Relate

### Smart Relate Documentation
```markdown
## Cooper Copilot Smart Relate

### Description
Automatic linking of related assets using AI.

### Supported Links
| Source | Target | Example |
|--------|--------|---------|
| Intune Device | M365 User | Device ↔ Assigned User |
| Configuration | Contact | Server ↔ Administrator |

### Prerequisites
- Microsoft Integration enabled
- Device Sync enabled
- Contact Sync enabled

### Manual Additions
- Automatic links can be edited
- Additional Related Items can be added manually
```

## Smart Assist

### Smart Assist Documentation
```markdown
## Cooper Copilot Smart Assist

### Description
AI-powered analysis and cleanup of documents.

### Features
| Feature | Description |
|---------|-------------|
| Stale Documents | Identifies outdated documents |
| Redundant Documents | Finds duplicates |
| Doc Health | Evaluates document quality |

### Cleanup Recommendations
- Archive outdated documents
- Merge duplicates
- Complete missing content

### Actions
| Recommendation | Action |
|----------------|--------|
| Outdated | Archive/Update |
| Duplicate | Merge/Delete |
| Incomplete | Complete |
```

## @relate Links in Documents

### @relate Documentation
```markdown
## @relate Functionality

### Usage
1. Type "@relate" in document text field
2. Select asset type
3. Filter globally or by organization
4. Select asset
5. Link is created

### Supported Asset Types
- Configurations
- Passwords
- Flexible Assets
- Documents
- Contacts

### Example
@relate [Configuration: Server01] creates automatic link
```

## Cooper Insights (KaseyaOne)

### KaseyaOne Cooper Insights
```markdown
## Cooper Insights: Password SafeShare

### Description
Cooper Insight to promote SafeShare usage.

### Criteria
- Insight displayed when: SafeShare not enabled
- Insight completed when: At least one password shared

### Additional Insights
[Document additional Cooper Insights here]
```

## Feature Deactivation

### Document Deactivation
```markdown
## Cooper Copilot Deactivation

### Reasons for Deactivation
- Privacy concerns
- Compliance requirements
- Manual control preferred

### Procedure
1. Admin > Settings > Cooper Copilot
2. Set toggle to "Off"
3. Save

### Impact
- Smart Relate disabled
- SOP Generator unavailable
- Smart Assist unavailable
- Existing links remain intact
```

## Best Practices

### Cooper Copilot Best Practices
```markdown
## Best Practices: Cooper Copilot

### SOP Generator
- Test run before production use
- Review sensitive data before publishing
- Check screenshots for visibility
- Plan regular SOP updates

### Smart Relate
- Review automatic links regularly
- Make manual corrections for incorrect links
- Ensure integration prerequisites

### Smart Assist
- Schedule regular cleanup cycles
- Prefer archiving over deletion
- Inform team about cleanup activities
```

## Checklist Cooper Copilot

### Activation
- [ ] Cooper Copilot enabled
- [ ] Team informed about features
- [ ] Privacy review completed

### SOP Generator
- [ ] Chrome Extension installed
- [ ] Test document created
- [ ] Workflow tested

### Smart Relate
- [ ] Microsoft Integration enabled
- [ ] Links reviewed

### Smart Assist
- [ ] Stale Documents review scheduled
- [ ] Cleanup strategy defined
