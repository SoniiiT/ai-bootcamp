# Rule 05: Security and Access Control

## Purpose
This rule defines documentation standards for ITGlue security features, permissions, and access controls.

## Documenting User Roles

### Role Hierarchy
- **Administrator**: Full access to all settings and data
- **Manager**: Administrative tasks without destructive actions
- **Editor**: Create, edit, delete with asset permissions
- **Creator**: Create and edit without delete rights
- **Read-only**: View-only access
- **Lite**: Read access to maximum 5 organizations (free)

### Documentation Format
```markdown
## User Role: [Role Name]

### Permissions
- Can create organizations: Yes/No
- Can manage users: Yes/No
- Can generate API keys: Yes/No
- Can export data: Yes/No
- Vault access: Yes/No

### Restrictions
- [Specific limitations]

### Use Cases
- [Typical usage scenarios]
```

## Group Documentation

### Required Fields
- Group name
- Description/purpose
- Assigned organizations
- Members
- Sidebar configuration (if customized)

### Documentation Structure
```markdown
## Group: [Group Name]

### Overview
| Property | Value |
|----------|-------|
| Creation Date | [Date] |
| Owner | [Person] |
| Member Count | [Count] |

### Organization Access
- [x] Organization A (full access)
- [x] Organization B (full access)
- [ ] Organization C (no access)

### Sidebar Customizations
- Visible sections: [List]
- Hidden sections: [List]
```

## Multi-Factor Authentication (MFA)

### MFA Enforcement Documentation
- Enforced MFA for all users
- MFA exemption list
- Recovery code procedures
- Authenticator app requirements

### Documentation Template
```markdown
## MFA Configuration

### Enforcement Status
- [x] MFA enforced for all users
- Exempted users: [List or "None"]

### Recovery Procedures
1. Use recovery code (one-time use)
2. Contact administrator for MFA reset
3. If only administrator: Contact Kaseya Support

### Authenticator Apps
- Recommended: Google Authenticator, Microsoft Authenticator
- Time synchronization required
```

## Vault Security

### Vault Documentation
- Passphrase requirements
- Users with vault access
- Vaulted passwords vs. regular passwords
- Access revocation procedures

### Important Notes
- Users with vault access must have access revoked before deletion
- Vaulted passwords CANNOT be shared via SafeShare
- User-specific passphrases required

## IP Access Controls

### Documentation Format
```markdown
## IP Access Control

### Configuration
- Status: Enabled/Disabled
- Mode: Allow all IPs / Allow specific IPs

### Allowed IP Addresses
| IP/Range | Description | Added On |
|----------|-------------|----------|
| 192.168.1.0/24 | Office network | [Date] |
| 10.0.0.5 | VPN Gateway | [Date] |

### Limits
- Maximum: 200 single IPs or ranges
- Format: Single IPs or CIDR notation
```

## Asset-Level Permissions

### Documenting Asset Security
- Who can view the asset?
- Who can edit the asset?
- Group-based vs. individual permissions

### Example Format
```markdown
## Asset Permission: [Asset Name]

### Visibility
- Groups: [Group list]
- Individual users: [User list]

### Edit Rights
- Groups: [Group list]
- Individual users: [User list]

### Inheritance
- Inherits from: [Parent asset/organization]
```

## SSO Exemption

### SSO Exception Documentation
```markdown
## Enforced SSO Override

### Users with Direct Authentication
| User | Reason | Approved By | Date |
|------|--------|-------------|------|
| [Email] | [Justification] | [Admin] | [Date] |

### Implications
- These users can ONLY log in with direct authentication
- SSO is not available for these users
```

## Security Documentation Checklist

- [ ] All user roles documented
- [ ] Group assignments current
- [ ] MFA status verified for all users
- [ ] Vault access rights documented
- [ ] IP whitelist updated
- [ ] Asset permissions reviewed
- [ ] SSO configuration documented
- [ ] Recovery procedures documented
