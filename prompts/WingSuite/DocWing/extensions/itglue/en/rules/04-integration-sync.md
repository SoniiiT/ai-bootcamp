# Rule 4 - Integration and Synchronization

## Overview

ITGlue supports numerous integrations for automatic data synchronization. This rule describes best practices for documenting integration data.

## PSA Integration

### Synchronized Data Types

| Data Type | Direction | Description |
|-----------|-----------|-------------|
| Organizations | PSA → ITGlue | Customer organizations |
| Configurations | Bidirectional | Devices/Assets |
| Contacts | Bidirectional | Contact persons |
| Locations | PSA → ITGlue | Sites |
| Tickets | PSA → ITGlue | Service tickets |

### Documentation Best Practices

**Document sync status:**
```markdown
## Integration Status

| System | Status | Last Sync | Notes |
|--------|--------|-----------|-------|
| Autotask | ✅ Active | [Date/Time] | Bidirectional |
| Datto RMM | ✅ Active | [Date/Time] | Device Sync |
| Microsoft 365 | ✅ Active | [Date/Time] | User/License Sync |
```

**Document sync configuration:**
```markdown
## Autotask Sync Configuration

### Account Types (synchronized)
- Customer
- Partner

### Configuration Item Types
- Server
- Workstation
- Network Device
- Printer

### Not synchronized
- Internal (Internal test organizations)
- Prospect (Prospects)
```

## RMM Integration

### Datto RMM Documentation

**Matching documentation:**
```markdown
## Datto RMM Site Matching

| ITGlue Organization | Datto RMM Site | Status |
|---------------------|----------------|--------|
| Client A Inc | Client_A | ✅ Matched |
| Client B Corp | ClientB-Site | ✅ Matched |
| Client C | [No Match] | ⚠️ Unmatched |
```

**Device sync status:**
```markdown
## Configuration Sync

### Automatically synchronized fields
- Name
- Serial Number
- Manufacturer
- Model
- Operating System
- IP Address
- MAC Address

### Manually maintained fields
- Asset Tag
- Assigned Contact
- Location
- Notes
- Related Items
```

## Microsoft Integration

### Microsoft 365 / GDAP

**Tenant documentation:**
```markdown
## Microsoft 365 Tenant

### Tenant Information
- **Tenant Name:** [Name]
- **Tenant ID:** [GUID]
- **GDAP Status:** ✅ Active
- **Last Sync:** [Date/Time]

### Synchronized Data
- [ ] Users → Contacts
- [ ] Licenses → Flexible Assets
- [ ] Groups → Flexible Assets

### Permissions
- Directory Reader
- Security Reader
- License Administrator
```

## Network Glue

### Setup Documentation

```markdown
## Network Glue Configuration

### Collector Information
- **Collector Name:** [Name]
- **Version:** [X.X.X]
- **Installed on:** [Server/Device]
- **Status:** ✅ Online

### Scan Configuration
- **IP Ranges:** [Subnet list]
- **Scan Frequency:** [Hourly/Daily]
- **SNMP Version:** [v2c/v3]
- **AD Integration:** [Yes/No]

### Credentials
- **SNMP Community:** [Reference to Password]
- **AD Service Account:** [Reference to Password]
```

### Matching Rules

```markdown
## Network Glue Matching Rules

### Automatic Matching
Devices are automatically matched based on:
1. Serial Number
2. MAC Address
3. Hostname

### Manual Matching Required
- Virtual machines without unique identifiers
- Network devices without serial number
- Printers without MAC address
```

## Documenting Sync Errors

### Error Log Template

```markdown
## Integration Sync Log

### [Date] - [Integration Name]

#### Errors
| Time | Error Type | Affected Object | Action |
|------|------------|-----------------|--------|
| [Time] | Matching Failed | [Object] | Match manually |
| [Time] | Sync Error | [Object] | Contacted support |

#### Resolved Issues
- [Description of problem and solution]

#### Open Items
- [ ] [Open item 1]
- [ ] [Open item 2]
```

## Two-Way Sync Rules

### Define Source of Truth

```markdown
## Data Ownership

| Data Field | Source of Truth | Note |
|------------|-----------------|------|
| Organization Name | PSA | PSA wins on conflict |
| Contact Info | PSA | Automatically overwritten |
| Passwords | ITGlue | Managed only in ITGlue |
| Notes | ITGlue | Not synchronized |
| Asset Tag | ITGlue | Manually maintained in ITGlue |
```

### Conflict Resolution

```markdown
## Sync Conflict Resolution

### Rule 1: PSA wins for base data
- Organization Name
- Contact First/Last Name
- Configuration Serial Number

### Rule 2: ITGlue wins for documentation
- Notes/Comments
- Related Items
- Embedded Passwords
- Documents
```
