# Rule 15: API Usage & Exports

## Purpose
This rule defines standards for documenting ITGlue API usage, data exports, and backup strategies.

## API Basics

### Document API Overview
```markdown
## ITGlue API: Overview

### Basic Information
| Property | Value |
|----------|-------|
| Base URL | https://api.itglue.com |
| EU Base URL | https://api.eu.itglue.com |
| AU Base URL | https://api.au.itglue.com |
| API Version | v1 |
| Format | JSON |
| Authentication | API Key (Header) |

### Rate Limits
| Limit | Value |
|-------|-------|
| Requests per minute | [Limit] |
| Bulk Requests | [Limit] |
```

### API Key Documentation
```markdown
## API Key Management

### Key Creation
| Property | Value |
|----------|-------|
| Key Name | [Descriptive Name] |
| Created | [Date] |
| Created By | [User] |
| Permissions | [Read/Write] |
| Purpose | [Purpose] |

### Security
- Never commit keys to code
- Use environment variables
- Regular rotation
- Delete unused keys

### Header Format
```
x-api-key: [API_KEY]
Content-Type: application/vnd.api+json
```
```

## Pagination

### Document Pagination
```markdown
## API Pagination

### Standard Pagination
| Parameter | Description | Default |
|-----------|-------------|---------|
| page[size] | Entries per page | 50 |
| page[number] | Page number | 1 |

### Response Metadata
```json
{
  "meta": {
    "current-page": 1,
    "next-page": 2,
    "prev-page": null,
    "total-pages": 10,
    "total-count": 500
  }
}
```

### Retrieve All Pages
```powershell
$page = 1
$allData = @()
do {
    $response = Invoke-RestMethod -Uri "$baseUrl?page[number]=$page"
    $allData += $response.data
    $page++
} while ($response.meta.'next-page')
```
```

## Filtering & Sorting

### Filter Documentation
```markdown
## API Filtering

### Standard Filters
| Parameter | Example | Description |
|-----------|---------|-------------|
| filter[id] | 123 | Filter by ID |
| filter[name] | Server01 | Filter by name |
| filter[organization_id] | 100 | Filter by organization |
| filter[updated_at] | 2024-01-01 | Filter by date |

### Date Filters (RFC3339)
```
filter[updated_at]=2024-01-01T00:00:00Z
filter[created_at]=gt:2024-01-01T00:00:00Z
```

### Sorting
| Parameter | Description |
|-----------|-------------|
| sort=name | Ascending by name |
| sort=-name | Descending by name |
| sort=created_at | By creation date |
```

### Filter Combinations
```markdown
## Complex Filters

### Example: Recent Configurations
```
/configurations
  ?filter[organization_id]=100
  &filter[configuration_status_id]=active
  &sort=-updated_at
  &page[size]=100
```

### Example: Passwords with Expiry
```
/passwords
  ?filter[password_expiring]=true
  &filter[organization_id]=100
  &sort=password_expires_at
```
```

## PowerShell Backup Scripts

### Backup Script Documentation
```markdown
## ITGlue Backup with PowerShell

### Prerequisites
- PowerShell 5.1 or higher
- API key with read permissions
- Sufficient storage space

### Basic Script
```powershell
# Configuration
$apiKey = $env:ITGLUE_API_KEY
$baseUrl = "https://api.itglue.com"
$headers = @{
    "x-api-key" = $apiKey
    "Content-Type" = "application/vnd.api+json"
}

# Export organizations
$orgs = Invoke-RestMethod -Uri "$baseUrl/organizations" -Headers $headers
$orgs.data | ConvertTo-Json -Depth 10 | Out-File "organizations.json"
```

### Complete Backup Script
[Link to complete script]
```

### Export Types
```markdown
## Exportable Resources

### Resource Matrix
| Resource | Endpoint | Export Available |
|----------|----------|------------------|
| Organizations | /organizations | ✅ |
| Configurations | /configurations | ✅ |
| Passwords | /passwords | ✅ |
| Documents | /documents | ✅ |
| Flexible Assets | /flexible_assets | ✅ |
| Contacts | /contacts | ✅ |
| Locations | /locations | ✅ |
| Domains | /domains | ✅ |

### Export Format
- JSON (API native)
- CSV (after conversion)
```

## CSV Export (UI)

### Document UI Export
```markdown
## ITGlue CSV Export (User Interface)

### Available Exports
| Area | Export Option |
|------|---------------|
| Configurations | Filter → Export CSV |
| Contacts | Filter → Export CSV |
| Passwords | Limited (Security) |
| Documents | Not directly |

### Export Steps
1. Navigate to asset list
2. Apply filters
3. Click "Export"
4. Choose format (CSV)
5. Download

### Limitations
- Password values not exportable (UI)
- Large data volumes: Prefer API
- Embedded Passwords: API only
```

## Backup Strategy

### Document Backup Concept
```markdown
## ITGlue Backup Strategy

### Backup Frequency
| Data Type | Frequency | Retention |
|-----------|-----------|-----------|
| Organizations | Weekly | 90 days |
| Configurations | Daily | 30 days |
| Passwords | Daily | 90 days |
| Documents | Weekly | 90 days |
| Flexible Assets | Daily | 30 days |

### Backup Location
| Location | Encryption | Access |
|----------|------------|--------|
| Azure Blob | AES-256 | RBAC |
| AWS S3 | Server-side | IAM |
| On-Premises | BitLocker | AD |

### Backup Validation
- Monthly restore tests
- Integrity checks
- Sample validation
```

## API Documentation Templates

### Endpoint Documentation
```markdown
## API Endpoint: [Endpoint Name]

### Overview
| Property | Value |
|----------|-------|
| Method | GET/POST/PATCH/DELETE |
| Endpoint | /[resource] |
| Authentication | API Key |

### Request
```http
GET /organizations HTTP/1.1
Host: api.itglue.com
x-api-key: [API_KEY]
Content-Type: application/vnd.api+json
```

### Response
```json
{
  "data": [...],
  "meta": {...},
  "links": {...}
}
```

### Error Codes
| Code | Meaning |
|------|---------|
| 200 | Success |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Rate Limit |
```

## Automated Exports

### Document Automation
```markdown
## Automated ITGlue Exports

### Scheduled Tasks (Windows)
```powershell
# Create task
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" `
    -Argument "-File C:\Scripts\ITGlue-Backup.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At 2am
Register-ScheduledTask -TaskName "ITGlue Backup" `
    -Action $action -Trigger $trigger
```

### Cron (Linux)
```bash
# Daily at 2 AM
0 2 * * * /usr/local/bin/itglue-backup.sh
```

### Azure Automation
- Create runbook
- Define schedule
- Store key in Key Vault
```

## Error Handling

### Document Error Handling
```markdown
## API Error Handling

### Retry Logic
```powershell
$maxRetries = 3
$retryCount = 0
do {
    try {
        $response = Invoke-RestMethod -Uri $url -Headers $headers
        break
    }
    catch {
        $retryCount++
        if ($retryCount -ge $maxRetries) { throw }
        Start-Sleep -Seconds (2 * $retryCount)
    }
} while ($retryCount -lt $maxRetries)
```

### Rate Limit Handling
- Detect 429 response
- Respect Retry-After header
- Exponential backoff

### Logging
- Log all API calls
- Document errors
- Collect performance metrics
```

## API Best Practices

### API Best Practices
```markdown
## ITGlue API: Best Practices

### Performance
- Use pagination (don't fetch all data at once)
- Use filters to reduce data volume
- Avoid parallel requests (rate limits)

### Security
- Don't hardcode API keys
- Store secrets in vault
- Least privilege for API keys

### Maintainability
- Version control scripts
- Keep documentation current
- Log changes

### Monitoring
- Monitor API availability
- Verify backup success
- Alert on failed calls
```

## Checklist API & Exports

### API Setup
- [ ] API key created
- [ ] Key securely stored
- [ ] Base URL correct
- [ ] Test request successful

### Backup Setup
- [ ] Backup script created
- [ ] Automation configured
- [ ] Storage location defined
- [ ] Encryption active

### Monitoring
- [ ] Backup success verified
- [ ] Errors reported
- [ ] Restore tests scheduled
