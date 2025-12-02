# Rule 10: PSA Integration

## Purpose
This rule defines standards for documenting PSA integrations (Autotask, ConnectWise, Kaseya BMS, Pulseway PSA).

## General PSA Documentation

### Integration Overview
```markdown
## PSA Integration: [PSA Name]

### Connection Information
| Property | Value |
|----------|-------|
| PSA Type | Autotask/ConnectWise/BMS/Pulseway |
| API URL | [URL] |
| Sync Status | Active/Paused |
| Last Synchronization | [Date/Time] |

### Sync Method
- [ ] One-Way Sync (PSA → ITGlue)
- [ ] Two-Way Sync (PSA ↔ ITGlue)

### Sync Settings
| Data Type | Sync Enabled |
|-----------|--------------|
| Organizations | Yes/No |
| Contacts | Yes/No |
| Configurations | Yes/No |
| Locations | Yes/No |
```

## Autotask Integration

### Autotask Documentation
```markdown
## Autotask Integration

### API Configuration
| Setting | Value |
|---------|-------|
| Integration Code | [Code] |
| Username | [API User] |
| Secret | [stored as password] |
| Zone | [Zone URL] |

### Ticket Rules
| Rule Name | Conditions | Actions |
|-----------|------------|---------|
| [Name] | [Criteria] | [Display IT Glue documents] |

### LiveLinks
| Link Type | URL Pattern |
|-----------|-------------|
| IT Glue Org | https://[subdomain].itglue.com/links/autotask/org/ |
| IT Glue Config | https://[subdomain].itglue.com/links/autotask/config/ |
```

### Ticket Insights Documentation
```markdown
## Autotask Ticket Insights

### Available Insights
| Insight | Description |
|---------|-------------|
| IT Glue Documents | Relevant documents |
| IT Glue Passwords | Relevant passwords |
| IT Glue Flexible Assets | Relevant flexible assets |

### Configuration
- Ticket Category: [Category]
- Enabled for Security Levels: [List]
- Enabled for Departments: [List]
```

## ConnectWise Integration

### ConnectWise Documentation
```markdown
## ConnectWise Manage Integration

### API Configuration
| Setting | Value |
|---------|-------|
| Company ID | [ID] |
| Public Key | [Key] |
| Private Key | [stored as password] |
| API URL | [URL] |

### Security Roles
- Required role for ITGlue API access
- Permissions for Tickets, Companies, Configurations

### Pod Configuration
| Pod | Enabled |
|-----|---------|
| Configurations | Yes/No |
| Passwords | Yes/No |
| Documents | Yes/No |
```

## Kaseya BMS Integration

### BMS Documentation
```markdown
## Kaseya BMS Integration

### API Configuration
| Setting | Value |
|---------|-------|
| API Key | [stored as password] |
| Tenant | [Tenant Name] |

### Two-Way Sync (BMS-specific)
- Contact Sync: Yes/No
- Organizations Sync: Yes/No
- Configurations Sync: Yes/No
```

## Two-Way Sync

### Two-Way Sync Documentation
```markdown
## Two-Way Sync: [PSA Name]

### Enabled Fields
| Field | Sync Direction |
|-------|----------------|
| Organization Name | PSA ↔ ITGlue |
| Contact Email | PSA ↔ ITGlue |
| Contact Phone | PSA ↔ ITGlue |
| Location Address | PSA ↔ ITGlue |

### Sync Frequency
- ITGlue → PSA: Immediately after change
- PSA → ITGlue: Regular + manual

### Conflict Handling
- Source of Truth: [PSA/ITGlue]
- Last change wins: Yes/No
```

## Field Mappings

### Mapping Documentation
```markdown
## Field Mappings: [PSA Name]

### Organizations
| PSA Field | ITGlue Field |
|-----------|--------------|
| Company Name | Organization Name |
| Company ID | External ID |
| Status | Organization Status |

### Contacts
| PSA Field | ITGlue Field |
|-----------|--------------|
| First Name | First Name |
| Last Name | Last Name |
| Email | Primary Email |
| Phone | Office Phone |
| Mobile | Mobile Phone |

### Configurations
| PSA Field | ITGlue Field |
|-----------|--------------|
| Configuration Name | Name |
| Serial Number | Serial Number |
| Asset Tag | Asset Tag |
| Manufacturer | Manufacturer |
| Model | Model |
```

## Sync Issue Documentation

### Error Handling
```markdown
## Sync Error Log

### Common Issues
| Issue | Cause | Solution |
|-------|-------|----------|
| Duplicates | Matching failed | Manual matching |
| Missing fields | API permission | Check permissions |
| Timeout | Too many records | Adjust schedule |

### Monitoring
- Last successful sync: [Date]
- Failed syncs: [Count]
- Warnings: [Details]
```

## Disconnect Procedures

### Disconnect Integration
```markdown
## PSA Integration Disconnect

### Option 1: Disconnect with Data Retention
- Connection to PSA is terminated
- All synced data remains in ITGlue
- Data can be used with other integrations

### Option 2: Disconnect with Data Deletion
- WARNING: Irreversible!
- All synced organizations are deleted
- Passwords, documents, flexible assets are deleted
- Only activatable via Support

### Before Disconnect
- [ ] Create full backup
- [ ] Document current configuration
- [ ] Inform affected users
```

## PSA Integration Checklist

### Setup
- [ ] API credentials configured
- [ ] Permissions verified
- [ ] Sync settings defined
- [ ] Two-way sync enabled (if desired)
- [ ] Field mappings documented

### Ongoing Operations
- [ ] Check sync status regularly
- [ ] Monitor error log
- [ ] Perform matching regularly
- [ ] Clean up duplicates

### Documentation
- [ ] API credentials stored securely
- [ ] Configuration documented
- [ ] Ticket rules documented (Autotask)
- [ ] LiveLinks/Pods documented
