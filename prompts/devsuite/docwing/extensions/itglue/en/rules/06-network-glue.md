# Rule 06: Network Glue Documentation

## Purpose
This rule defines standards for documenting Network Glue features, network discovery, and device management.

## Network Glue Collector Deployment

### Collector Documentation
```markdown
## Network Glue Collector: [Location/Client]

### Collector Information
| Property | Value |
|----------|-------|
| Collector Name | [Name] |
| Installation Location | [Server/Workstation] |
| IP Address | [IP] |
| Version | [Version] |
| Last Sync | [Date/Time] |
| Status | Active/Inactive |

### Network Configuration
- Domain-based: Yes/No
- SNMP enabled: Yes/No
- SNMP Version: v1/v2c/v3
- WMI enabled: Yes/No
```

## SNMP Configuration Documentation

### Vendor-Specific SNMP Requirements
- **Meraki**: Dashboard API
- **Cisco**: SNMP v2c/v3 with specific community strings
- **HP/Aruba**: SNMP v2c standard
- **Fortinet**: SNMP with FortiOS-specific OIDs
- **SonicWall**: SNMP enabled via management interface
- **Sophos**: SNMP with XG/UTM-specific settings

### SNMP Documentation Template
```markdown
## SNMP Configuration: [Device Name]

### General Settings
| Parameter | Value |
|-----------|-------|
| SNMP Version | v2c / v3 |
| Community String | [stored as password] |
| Port | 161 |

### SNMPv3 (if used)
| Parameter | Value |
|-----------|-------|
| Username | [Username] |
| Auth Protocol | MD5/SHA |
| Privacy Protocol | DES/AES |
| Security Level | noAuthNoPriv/authNoPriv/authPriv |
```

## Network Diagrams

### Automatic Topology Documentation
- Physical connections between devices
- Switch port mappings
- VLAN assignments
- Router/gateway relationships

### Manual Additions
```markdown
## Network Topology: [Location]

### Diagram Information
- Generated on: [Date]
- Last update: [Date]
- Device count: [Count]

### Notes
- [Manual annotations about connections]
- [Unrecognized devices]
- [Special configurations]
```

## Device Matching

### Matching Logic Documentation
1. Primary MAC address + Serial number (exact)
2. Primary MAC address only
3. Serial number only
4. Device name (manual matching)

### Documentation Format
```markdown
## Device Matching: [Organization]

### Matched Devices
| ITGlue Config | Network Glue Device | Match Method |
|---------------|---------------------|--------------|
| [Config Name] | [Device Name] | MAC/Serial/Name |

### Unmatched Devices
| Device | Reason | Action |
|--------|--------|--------|
| [Name] | [Why not matched] | Ignore/Manual match/Create new |
```

## Windows Domain Networks

### Active Directory Integration
```markdown
## AD Integration: [Domain]

### Configuration
- Domain Controller: [DC Name/IP]
- GPO for Collector: Yes/No
- WMI enabled: Yes/No
- Remote Registry: Yes/No

### GPO Settings
- Computer Configuration > Administrative Templates > Network
- WMI firewall rules
- Remote Registry Service enabled

### Sync Information
- AD Users: [Count]
- AD Computers: [Count]
- AD Groups: [Count]
- Last Synchronization: [Date]
```

## Multiple Subnets

### Subnet Documentation
```markdown
## Network Subnets: [Location]

### Subnet Overview
| Subnet | VLAN | Description | SNMP-capable |
|--------|------|-------------|--------------|
| 192.168.1.0/24 | 10 | Servers | Yes |
| 192.168.2.0/24 | 20 | Clients | No |
| 10.0.0.0/24 | 100 | Management | Yes |

### Routing
- Gateway: [IP]
- DNS Servers: [IPs]
```

## Firewall Rules for Network Glue

### Required Ports
```markdown
## Firewall Rules: Network Glue

### Outbound (Collector → Internet)
| Port | Protocol | Destination | Purpose |
|------|----------|-------------|---------|
| 443 | TCP | *.itglue.com | API communication |

### Internal (Collector → Network)
| Port | Protocol | Purpose |
|------|----------|---------|
| 161 | UDP | SNMP |
| 135 | TCP | WMI/RPC |
| 445 | TCP | SMB |
| 5985/5986 | TCP | WinRM |
```

## Password Rotation (AD)

### Scheduled Password Rotation
```markdown
## Password Rotation: [Domain]

### Configuration
| Setting | Value |
|---------|-------|
| Rotation Frequency | [1-365 days] |
| Enabled for | On-Prem AD / Entra ID / Both |
| Bulk activation | Yes/No |

### Affected Accounts
- Service Accounts: [List]
- Admin Accounts: [List]
- Excluded: [List]

### Notifications
- Success: [Email/Webhook]
- Failure: [Email/Webhook]
```

## Network Device Types

### Configuration Types Documentation
- Managed Server
- Managed Workstation
- Firewall
- Switch
- Router
- Access Point
- Printer
- Other

### Automatic Discovery
```markdown
## Auto-Discovery: [Network]

### Discovered Device Types
| Type | Count | Auto-classified |
|------|-------|-----------------|
| Server | [X] | Yes/No |
| Workstation | [X] | Yes/No |
| Network Device | [X] | Yes/No |
```

## Network Glue Documentation Checklist

- [ ] Collector installed and documented
- [ ] SNMP credentials stored
- [ ] Network ranges defined
- [ ] Device matching completed
- [ ] Topology diagram verified
- [ ] AD integration configured (if applicable)
- [ ] Firewall rules documented
- [ ] Password rotation configured (if needed)
