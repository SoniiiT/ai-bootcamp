# Rule 07: Microsoft Integration

## Purpose
This rule defines documentation standards for Microsoft/Azure integration in ITGlue, including Intune, Entra ID, and GDAP.

## Integration Setup Documentation

### Azure App Registration
```markdown
## Microsoft Integration: [Tenant Name]

### App Registration
| Property | Value |
|----------|-------|
| Application (Client) ID | [GUID] |
| Directory (Tenant) ID | [GUID] |
| Client Secret | [stored as ITGlue password] |
| Secret Expiration | [Date] |

### Redirect URI
- NA: https://[subdomain].itglue.com/microsofts
- EU: https://[subdomain].eu.itglue.com/microsofts
- AU: https://[subdomain].au.itglue.com/microsofts
```

## API Permissions

### Required Permissions Documentation
```markdown
## API Permissions: Microsoft Graph

### Delegated Permissions
| Permission | Type | Status |
|------------|------|--------|
| User.Read.All | Delegated | ✓ Granted |
| Device.Read.All | Delegated | ✓ Granted |
| Directory.Read.All | Delegated | ✓ Granted |
| BitLockerKey.Read.All | Delegated | ✓ Granted |

### Application Permissions
| Permission | Type | Status |
|------------|------|--------|
| User.Read.All | Application | ✓ Granted |
| Device.Read.All | Application | ✓ Granted |

### Admin Consent
- Granted by: [Admin Name]
- Date: [Date]
```

## GDAP Configuration

### Granular Delegated Admin Privileges
```markdown
## GDAP Setup: [Partner Tenant]

### Service Account
| Property | Value |
|----------|-------|
| Username | [UPN] |
| Assigned to | AdminAgents Group |
| Role | Global Administrator |
| MFA enabled | Yes (required) |

### Security Group
| Property | Value |
|----------|-------|
| Group Name | [Name] |
| Group Type | Security |
| Members | Service Account |

### GDAP Relationships
| Customer/Tenant | Assigned Role | Status |
|-----------------|---------------|--------|
| [Tenant A] | Global Administrator | Active |
| [Tenant B] | Privileged Role Admin | Active |
```

## Field Mappings

### Microsoft → ITGlue Mappings
```markdown
## Field Mappings: Microsoft Integration

### Organizations
| Microsoft Field | ITGlue Field |
|-----------------|--------------|
| Tenant Name | Organization Name |
| Address | Location Address |
| Phone | Location Phone |

### Contacts
| Microsoft Field | ITGlue Field |
|-----------------|--------------|
| First Name + Last Name | Name |
| Job Title | Title |
| Phone | Office Phone |
| Mobile | Mobile Phone |
| User Name | Primary Email |
| Alias | Mailbox Aliases |

### Configurations (Intune)
| Microsoft Field | ITGlue Field |
|-----------------|--------------|
| Device Type | Configuration Type |
| Device Name | Hostname |
| Serial Number | Serial Number |
| Manufacturer | Manufacturer |
| Model | Model |
```

## Intune Device Sync

### Device Documentation
```markdown
## Intune Devices: [Organization]

### Sync Settings
- Sync enabled: Yes/No
- Last synchronization: [Date]
- Device count: [Count]

### Device Matching
| Matching Rule | Priority |
|---------------|----------|
| MAC + Serial | 1 (highest) |
| MAC only | 2 |
| Serial only | 3 |
| Device Name | 4 (manual) |

### Copilot Smart Relate
- Automatic linking: Intune Device ↔ M365 User
```

## BitLocker Recovery Keys

### BitLocker Documentation
```markdown
## BitLocker Keys: [Organization]

### Prerequisites
- Network Glue Subscription: Required
- API Permission: BitLockerKey.Read.All (Delegated)
- Intune Device Sync: Enabled

### Folder Structure
- Folder: "BitLocker Keys (via Network Glue)"
- Category: BitLocker Keys (via Network Glue)
- Type: Auto-generated

### Key Information
| Field | Description |
|-------|-------------|
| Name | Device Name from Intune |
| Username | BitLocker Recovery Key ID |
| Password | 48-digit Recovery Key |
| Notes | Automatically populated |

### Restrictions
- Keys cannot be edited
- Archived keys for older versions
- Cannot be moved outside folder
- Password rotation not available
```

## Microsoft 365 Licenses

### License Sync Documentation
```markdown
## M365 Licenses: [Organization]

### Sync Configuration
- Sync license types: Yes/No
- Show assigned licenses: Yes/No

### License Overview
| License Type | Total | Assigned | Available |
|--------------|-------|----------|-----------|
| M365 Business Basic | [X] | [Y] | [Z] |
| M365 Business Standard | [X] | [Y] | [Z] |
```

## Entra ID Sync

### Sync Settings
```markdown
## Entra ID Sync: [Tenant]

### Configuration
| Setting | Value |
|---------|-------|
| Sync enabled | Yes/No |
| BitLocker Keys | Yes/No |
| License Information | Yes/No |

### Contact Sync
- All users: Yes/No
- Filtered users: [Filter criteria]
- Last synchronization: [Date]
```

## Matching Logic

### Organization Matching
```markdown
## Matching: Microsoft Tenants

### Automatic Matching
- Criteria: Exact Tenant Name ↔ Organization Name

### Manual Matching Required For
- Differing names
- New tenants without ITGlue counterpart
- Multi-tenant scenarios
```

## Microsoft Integration Checklist

- [ ] Azure App Registration created
- [ ] API Permissions configured
- [ ] Admin Consent granted
- [ ] GDAP set up (if partner)
- [ ] Service Account with MFA created
- [ ] Security Group configured
- [ ] Tenant matching completed
- [ ] Intune Device Sync enabled
- [ ] BitLocker Key Sync enabled
- [ ] Contact Sync configured
- [ ] Client Secret expiration documented
