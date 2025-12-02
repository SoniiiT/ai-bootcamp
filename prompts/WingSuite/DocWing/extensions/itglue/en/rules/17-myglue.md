# Rule 17: MyGlue Configuration

## Purpose
This rule defines standards for documenting MyGlue, the customer portal for ITGlue.

## MyGlue Overview

### MyGlue Basics
```markdown
## MyGlue: Overview

### What is MyGlue?
Customer portal for controlled access to ITGlue content.

### Main Functions
| Function | Description |
|----------|-------------|
| Password Management | Customers manage their own passwords |
| Document Access | Read shared documents |
| Self-Service | Reset passwords themselves |
| Collaboration | Shared documentation |

### Prerequisites
- MyGlue license activated
- Customer users created
- Permissions configured
```

## Five Deployment Scenarios

### Scenario 1: Password Management
```markdown
## MyGlue Scenario 1: Password Management

### Description
Customers use MyGlue exclusively for password management.

### Configuration
| Setting | Value |
|---------|-------|
| Password Access | ✅ Enabled |
| Document Access | ❌ Disabled |
| Configurations | ❌ Disabled |
| Flexible Assets | ❌ Disabled |

### User Permissions
| Permission | Value |
|------------|-------|
| View Passwords | ✅ |
| Create Passwords | Optional |
| Edit Passwords | Own only |
| Delete Passwords | ❌ |

### Benefits
- Minimal access
- Secure password management
- Simple setup
```

### Scenario 2: Internal Documentation
```markdown
## MyGlue Scenario 2: Internal Documentation

### Description
Customer IT team uses MyGlue for internal documentation.

### Configuration
| Setting | Value |
|---------|-------|
| Password Access | ✅ Enabled |
| Document Access | ✅ Enabled |
| Configurations | ✅ Enabled |
| Flexible Assets | Optional |

### Document Permissions
| Permission | Value |
|------------|-------|
| View Documents | ✅ |
| Create Documents | ✅ |
| Edit Documents | ✅ |
| Delete Documents | Optional |

### Usage
- IT team creates own documentation
- MSP can view content
- Shared knowledge base
```

### Scenario 3: Stakeholder Access
```markdown
## MyGlue Scenario 3: Stakeholder Access

### Description
Executives/stakeholders get read access to relevant information.

### Configuration
| Setting | Value |
|---------|-------|
| Password Access | ✅ Read Only |
| Document Access | ✅ Read Only |
| Configurations | ❌ Disabled |
| Dashboards | ✅ Enabled |

### User Permissions
| Permission | Value |
|------------|-------|
| View | ✅ |
| Edit | ❌ |
| Create | ❌ |
| Delete | ❌ |

### Typical Content
- Overview documents
- Network diagrams
- Compliance reports
- Important passwords (CEO)
```

### Scenario 4: Collaboration
```markdown
## MyGlue Scenario 4: Collaboration

### Description
Full collaboration between MSP and customer.

### Configuration
| Setting | Value |
|---------|-------|
| Password Access | ✅ Full |
| Document Access | ✅ Full |
| Configurations | ✅ Full |
| Flexible Assets | ✅ Enabled |

### Permissions
| Area | Customer | MSP |
|------|----------|-----|
| Passwords | CRUD | CRUD |
| Documents | CRUD | CRUD |
| Configs | CR | CRUD |
| Flex Assets | CR | CRUD |

### Best Practices
- Define clear responsibilities
- Enable change tracking
- Regular synchronization
```

### Scenario 5: Self-Service
```markdown
## MyGlue Scenario 5: Self-Service

### Description
End users can manage and reset passwords themselves.

### Configuration
| Setting | Value |
|---------|-------|
| Self-Service | ✅ Enabled |
| Password Reset | ✅ Enabled |
| MFA | ✅ Required |
| SSO | Optional |

### Self-Service Features
| Feature | Description |
|---------|-------------|
| Password View | See own passwords |
| Password Reset | Self-reset |
| OTP Generation | Time-based codes |
| SafeShare | Secure password sharing |

### Security
- MFA for all users
- Audit log for all access
- Limited password visibility
```

## MyGlue Branding

### Branding Documentation
```markdown
## MyGlue Branding

### Customizable Elements
| Element | Customization |
|---------|---------------|
| Logo | Upload custom logo |
| Colors | Primary/secondary colors |
| Favicon | Custom icon |
| Login Page | Background image |

### Branding Settings
| Property | Value |
|----------|-------|
| Logo URL | [URL] |
| Primary Color | [Hex Code] |
| Secondary Color | [Hex Code] |
| Custom Domain | [Domain] (optional) |

### Best Practices
- Logo size: max 200x50px
- Contrast colors for readability
- Consistency with corporate branding
```

## MyGlue SSO

### SSO for MyGlue
```markdown
## MyGlue SSO Configuration

### Supported Providers
| Provider | Configuration |
|----------|---------------|
| Azure AD / Entra ID | SAML 2.0 |
| Google Workspace | SAML 2.0 |
| Okta | SAML 2.0 |
| OneLogin | SAML 2.0 |

### SAML Settings
| Property | Value |
|----------|-------|
| Entity ID | https://myglue.[subdomain].itglue.com/ |
| ACS URL | https://myglue.[subdomain].itglue.com/users/saml/auth |
| Name ID | Email |

### Configuration
1. Create IdP SAML app
2. Configure ITGlue MyGlue SSO
3. Perform test login
4. Enable SSO-only (optional)
```

## MyGlue Users

### User Management
```markdown
## MyGlue User Management

### User Types
| Type | Description |
|------|-------------|
| Full Access | Full access to org |
| Limited | Restricted access |
| Read-Only | Read permissions only |

### Create User
| Field | Value |
|-------|-------|
| Name | [First and Last Name] |
| Email | [Email Address] |
| Role | [Role] |
| Organization | [Assigned Org] |
| MFA | Required/Optional |

### Invitation Process
1. Create user in ITGlue
2. Enable MyGlue access
3. Invitation email sent
4. User sets password
5. MFA setup
```

## MyGlue Security

### Security Documentation
```markdown
## MyGlue Security

### Security Measures
| Measure | Status |
|---------|--------|
| MFA | ✅ Enforced |
| SSO | ✅/❌ |
| IP Restriction | Optional |
| Session Timeout | [Minutes] |

### Access Levels
| Level | Description |
|-------|-------------|
| Organization | Access to org content |
| Folder | Folder-based access |
| Asset | Single asset |
| Field | Field-based access |

### Audit
- All access logged
- Password views trackable
- Changes documented
```

## MyGlue Documentation for Customers

### Customer Guide
```markdown
## MyGlue: End User Guide

### Getting Started
1. Open invitation email
2. Click "Activate Account"
3. Set password
4. Set up MFA (if required)

### Retrieve Passwords
1. Log into MyGlue
2. Click "Passwords"
3. Search for password
4. Click "View"
5. Copy if needed

### Create Password (if authorized)
1. Click "New Password"
2. Enter details
3. Save

### Use SafeShare
1. Select password
2. Click "Share"
3. Copy link
4. Send to recipient
```

## MyGlue Checklist

### Setup
- [ ] MyGlue license activated
- [ ] Branding configured
- [ ] SSO set up (optional)
- [ ] MFA policy defined

### Users
- [ ] User types defined
- [ ] Permissions configured
- [ ] Invitations sent
- [ ] MFA setup completed

### Content
- [ ] Visible content defined
- [ ] Folder structure customized
- [ ] Password categories configured
- [ ] Documents shared

### Training
- [ ] Customer guide created
- [ ] Training conducted
- [ ] Support contact communicated
