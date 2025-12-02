# Rule 08: Import and Export

## Purpose
This rule defines standards for documenting data import, export, and backup procedures in ITGlue.

## CSV Import

### Import Prerequisites Documentation
```markdown
## CSV Import Preparation

### File Requirements
- Format: CSV (Comma Separated Values)
- Encoding: UTF-8 recommended
- Delimiter: Comma (,) or Semicolon (;)
- Header Row: Required

### Regional Settings
| Region | Decimal Separator | Thousands Separator | List Separator |
|--------|-------------------|---------------------|----------------|
| US/UK | . | , | , |
| DE/EU | , | . | ; |

### Before Import
- [ ] Existing data backed up
- [ ] CSV file validated
- [ ] Column mapping verified
- [ ] Test record imported
```

### Import Types Documentation
```markdown
## Available Import Types

### Organizations
| ITGlue Field | CSV Column | Required |
|--------------|------------|----------|
| name | Organization Name | Yes |
| short_name | Short Name | No |
| description | Description | No |
| organization_status | Status | No |

### Contacts
| ITGlue Field | CSV Column | Required |
|--------------|------------|----------|
| first_name | First Name | Yes |
| last_name | Last Name | Yes |
| primary_email | Email | No |
| title | Job Title | No |

### Passwords
| ITGlue Field | CSV Column | Required |
|--------------|------------|----------|
| name | Password Name | Yes |
| username | Username | No |
| password | Password | Yes |
| url | URL | No |
| notes | Notes | No |

### Flexible Assets
| ITGlue Field | CSV Column | Required |
|--------------|------------|----------|
| organization_name | Organization | Yes |
| [field_name] | [Corresponding Column] | Depends |
```

## Flexible Asset Template Import

### Template Import Documentation
```markdown
## Flexible Asset Template Import

### Available Templates
| Template Name | Category | Fields |
|---------------|----------|--------|
| Application | Apps & Services | [Count] |
| LAN | Infrastructure | [Count] |
| Licensing | Administration | [Count] |

### Import Process
1. Admin > Flexible Asset Types
2. Import > Flexible Asset Template
3. Select template
4. Customize fields (optional)
5. Confirm import

### After Import
- Adjust sidebar position
- Configure permissions
- Create test record
```

## Data Export

### Export Documentation
```markdown
## Data Export: [Date]

### Scheduled Export
| Setting | Value |
|---------|-------|
| Schedule | Daily/Weekly/Monthly |
| Export Type | Full/Incremental |
| Destination | Email/Download |
| Recipients | [Email addresses] |

### Exported Data
- [ ] Organizations
- [ ] Contacts
- [ ] Configurations
- [ ] Passwords
- [ ] Documents
- [ ] Flexible Assets
- [ ] Domains
- [ ] SSL Certificates

### Export Format
| Data Type | Filename | Format |
|-----------|----------|--------|
| Passwords | passwords.csv | CSV |
| Organizations | organizations.csv | CSV |
| Configurations | configurations.csv | CSV |
```

## Backup Procedures

### Backup Documentation
```markdown
## ITGlue Backup Strategy

### Automatic Exports
| Frequency | Content | Retention |
|-----------|---------|-----------|
| Daily | Passwords | 30 days |
| Weekly | Full | 12 weeks |
| Monthly | Full | 12 months |

### API-based Backup (PowerShell)
```powershell
# Example script for documentation
$ApiKey = Get-Content "path/to/apikey.txt"
$BaseUrl = "https://api.itglue.com"
# Additional backup logic...
```

### Disaster Recovery
- Recovery Contact: Kaseya Support
- RTO: [Time]
- RPO: [Time]
- Last Test Recovery: [Date]
```

## Network Detective Import

### Network Detective Import
```markdown
## Network Detective Import

### Prerequisites
- Network Detective Scan completed
- Export in ITGlue format created
- Target organization exists in ITGlue

### Imported Data
| Data Type | Available |
|-----------|-----------|
| Computers | Yes |
| Network Devices | Yes |
| Software | Yes |
| Users | Yes |

### Post-Processing
- [ ] Verify data completeness
- [ ] Assign configuration types
- [ ] Create flexible assets
```

## OTP Secret Key Export

### OTP in Exports
```markdown
## OTP Export Notes

### Export Behavior
- OTP Secret Key is exported in passwords.csv
- Column: 'otp_secret_key'
- Only with manual/scheduled export

### Import Limitation
- OTP Secret Key import is NOT supported
- OTP must be manually reconfigured
```

## Import Error Handling

### Error Documentation
```markdown
## Import Error Log

### Common Errors
| Error | Cause | Solution |
|-------|-------|----------|
| Encoding Error | Wrong character encoding | Use UTF-8 |
| Field Not Found | Wrong column name | Check mapping |
| Duplicate | Entry already exists | Skip/Update |
| Organization Not Found | Target org missing | Create org first |

### Error Log Format
| Row | Field | Error | Value | Status |
|-----|-------|-------|-------|--------|
| 5 | email | Invalid format | "test@" | Skipped |
```

## Import/Export Checklist

### Before Import
- [ ] Backup created
- [ ] CSV file validated
- [ ] Encoding verified (UTF-8)
- [ ] Column mapping documented
- [ ] Test run completed

### After Import
- [ ] Data count verified
- [ ] Spot checks performed
- [ ] Error log evaluated
- [ ] Permissions set

### For Exports
- [ ] Export scope defined
- [ ] Schedule configured
- [ ] Recipients set up
- [ ] Storage location secured
- [ ] Retention policy documented
