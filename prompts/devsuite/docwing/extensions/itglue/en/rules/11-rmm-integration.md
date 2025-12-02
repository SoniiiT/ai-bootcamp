# Rule 11: RMM Integration

## Purpose
This rule defines standards for documenting RMM integrations (Datto RMM, VSA, ConnectWise RMM).

## General RMM Documentation

### Integration Overview
```markdown
## RMM Integration: [RMM Name]

### Connection Information
| Property | Value |
|----------|-------|
| RMM Type | Datto RMM/VSA/ConnectWise RMM |
| API Endpoint | [URL] |
| Sync Status | Active/Paused |
| Last Synchronization | [Date/Time] |

### Sync Behavior
- RMM data displayed as OVERLAY
- No write operations to ITGlue
- "Compare Data" available for comparison
```

## Datto RMM Integration

### Datto RMM Documentation
```markdown
## Datto RMM Integration

### API Configuration
| Setting | Value |
|---------|-------|
| API URL | [URL] |
| API Key | [stored as password] |
| API Secret | [stored as password] |

### Sync Settings
| Data Type | Enabled |
|-----------|---------|
| Sites → Organizations | Yes/No |
| Devices → Configurations | Yes/No |

### SOP Generator Integration
- Web Remote Sessions are captured
- Automatic SOP creation possible
```

## VSA Integration

### VSA Documentation
```markdown
## Kaseya VSA Integration

### API Configuration
| Setting | Value |
|---------|-------|
| VSA URL | [URL] |
| Authentication | PAT (Personal Access Token) |

### Personal Access Token (PAT)
| Setting | Value |
|---------|-------|
| Token Name | [Name] |
| Scope | REST API Read/Write |
| IP Whitelist | [IPs or empty] |
| Expiration Date | [Date] |

### Agent Procedures
- Call ITGlue data from procedures possible
- URL parameter passing
```

## Field Mappings

### RMM → ITGlue Mappings
```markdown
## Field Mappings: [RMM Name]

### Configurations
| RMM Field | ITGlue Field |
|-----------|--------------|
| Device Name | Hostname |
| Serial Number | Serial Number |
| MAC Address | Primary MAC |
| IP Address | Primary IP |
| Manufacturer | Manufacturer |
| Model | Model |
| OS | Operating System |
| Last Reboot | Last Reboot |
| CPU | CPU |
| RAM | RAM |
| Last Check-in | Last Check-in |
```

## Device Matching

### Matching Logic
```markdown
## Device Matching: [RMM Name]

### Automatic Matching
| Priority | Criteria |
|----------|----------|
| 1 | MAC address + Serial number |
| 2 | MAC address only |
| 3 | Serial number only |

### Manual Matching
- Devices with similar names are suggested
- Match/Ignore/Create actions available

### Matching Status
| Status | Meaning |
|--------|---------|
| Matched | Successfully linked |
| Suggested | Suggestion available |
| Unmatched | No match found |
| Ignored | Deliberately skipped |
```

## Compare Data Feature

### Data Comparison Documentation
```markdown
## Compare Data: Configurations

### Available Fields for Comparison
| Field | ITGlue Value | RMM Value |
|-------|--------------|-----------|
| Hostname | [Value] | [Value] |
| Serial | [Value] | [Value] |
| IP Address | [Value] | [Value] |

### Note
- RMM data is not written to ITGlue
- Displayed as overlay
- On discrepancy: RMM value is preferred
```

## Datto Networking

### Datto Networking Documentation
```markdown
## Datto Networking Integration

### Features
- Wireless SSID Auto-Documentation
- Network device sync
- Access point documentation

### Wireless Networks
| SSID | Security Type | Password |
|------|---------------|----------|
| [SSID] | WPA2-Personal | [As ITGlue Password] |

### Flexible Asset
- Type: Datto Networking Wireless
- Automatically created for WPA Personal/PSK
```

## Datto BCDR Integration

### BCDR Documentation
```markdown
## Datto BCDR Integration

### Features
- DR Runbook access from Organization Home
- Backup status display
- Device assignment

### Matching
- BCDR devices → ITGlue configurations
- Organization-level assignment

### Quick Access
- DR Runbook link on Organization Home
- Only visible for matched organizations
```

## Datto Endpoint Backup

### Endpoint Backup Documentation
```markdown
## Datto Endpoint Backup

### Features
- Backup status for PCs
- Organization sync
- Coverage display

### Integration
- SaaS Protection Coverage in org list
- Product Type display
```

## Organization Matching

### Organization Matching
```markdown
## Organization Matching: [RMM Name]

### Automatic Matching
- Exact name match required
- Pattern recognition for suggestions

### Matching Documentation
| RMM Org | ITGlue Org | Status | Method |
|---------|------------|--------|--------|
| [Name] | [Name] | Matched | Auto/Manual |

### Best Practices
- Integrate PSA first
- Match RMM data, don't create new
- Avoid duplicates
```

## Sync Methods

### Recommended Method
```markdown
## Sync Strategy: PSA + RMM

### Option B (Recommended)
1. Integrate RMM with PSA
2. Push RMM data to PSA
3. Integrate PSA with ITGlue
4. Integrate RMM with ITGlue
5. Use RMM data as overlay

### Benefits
- No duplicates
- Automatic matching
- Combined data view
```

## RMM Integration Checklist

### Setup
- [ ] API credentials configured
- [ ] PAT token created (VSA)
- [ ] Sync settings defined
- [ ] Organization matching completed
- [ ] Device matching completed

### Ongoing Operations
- [ ] Check sync status
- [ ] Match new devices
- [ ] Clean up orphaned devices
- [ ] Use Compare Data regularly

### Documentation
- [ ] API credentials stored
- [ ] Field mappings documented
- [ ] Matching rules documented
- [ ] Token expiration noted (VSA)
