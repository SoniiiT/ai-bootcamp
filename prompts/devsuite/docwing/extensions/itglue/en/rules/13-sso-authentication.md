# Rule 13: SSO & Authentication

## Purpose
This rule defines standards for documenting Single Sign-On (SSO) and authentication configurations in ITGlue.

## Supported SSO Providers

### SSO Provider Matrix
```markdown
## Supported SSO Providers

| Provider | SAML 2.0 | Configuration Guide |
|----------|----------|---------------------|
| Microsoft Azure AD / Entra ID | ✅ | Azure Enterprise Apps |
| Microsoft ADFS | ✅ | ADFS Relying Party Trust |
| Google Workspace | ✅ | Google Admin Console |
| OneLogin | ✅ | OneLogin Admin Portal |
| Okta | ✅ | Okta Admin Dashboard |
| Centrify | ✅ | Centrify Admin Portal |
| LastPass Enterprise | ✅ | LastPass Admin Console |
| Duo Access Gateway | ✅ | Duo Admin Panel |
| Passly | ✅ | Passly Admin Console |
| Other SAML 2.0 | ✅ | Generic SAML Configuration |
```

## ITGlue SAML Configuration

### Document SAML Settings
```markdown
## ITGlue SAML Configuration

### Service Provider (SP) Details
| Property | Value |
|----------|-------|
| Entity ID | https://[subdomain].itglue.com/ |
| ACS URL | https://[subdomain].itglue.com/users/saml/auth |
| Name ID Format | Email |
| Fingerprint Algorithm | SHA256 |

### Identity Provider (IdP) Details
| Property | Value |
|----------|-------|
| SSO URL | [IdP SSO URL] |
| Entity ID | [IdP Entity ID] |
| Certificate | [Certificate Fingerprint] |
| Fingerprint Algorithm | SHA256 |
```

### Fingerprint Calculation
```markdown
## SAML Certificate Fingerprint

### Windows PowerShell
```powershell
$cert = Get-PfxCertificate -FilePath "idp_certificate.cer"
$cert.Thumbprint
```

### OpenSSL (Linux/Mac)
```bash
openssl x509 -in idp_certificate.cer -fingerprint -sha256 -noout
```

### Notes
- Remove colons
- Use uppercase only
- Use SHA256 as standard
```

## Azure AD / Entra ID

### Azure AD SSO Documentation
```markdown
## Azure AD / Entra ID SSO Configuration

### Azure Enterprise Application
| Property | Value |
|----------|-------|
| App Name | ITGlue SSO |
| App Type | Enterprise Application (Non-Gallery) |
| SAML Mode | SAML-based Sign-on |

### Azure SAML Settings
| Azure Field | Value |
|-------------|-------|
| Identifier (Entity ID) | https://[subdomain].itglue.com/ |
| Reply URL (ACS) | https://[subdomain].itglue.com/users/saml/auth |
| Sign on URL | https://[subdomain].itglue.com/ |
| Relay State | (empty) |
| Logout URL | (optional) |

### Attributes & Claims
| Claim | Value |
|-------|-------|
| Unique User Identifier | user.mail |

### User Assignment
- Assign users/groups to app
- Enable "Assignment required"
```

## ADFS Configuration

### ADFS SSO Documentation
```markdown
## Microsoft ADFS SSO Configuration

### Relying Party Trust
| Property | Value |
|----------|-------|
| Display Name | ITGlue |
| Relying Party Identifier | https://[subdomain].itglue.com/ |
| SAML Endpoint | https://[subdomain].itglue.com/users/saml/auth |
| Binding | POST |

### Claim Rules
| Rule | Type | Value |
|------|------|-------|
| Email | Send LDAP Attributes | E-Mail → Name ID |

### Token Settings
- Token Encryption: Disabled
- Token Signing: SHA-256
```

## Google Workspace

### Google SSO Documentation
```markdown
## Google Workspace SSO Configuration

### SAML App Settings
| Property | Value |
|----------|-------|
| App Name | ITGlue |
| ACS URL | https://[subdomain].itglue.com/users/saml/auth |
| Entity ID | https://[subdomain].itglue.com/ |
| Name ID | Basic Information > Primary Email |
| Name ID Format | EMAIL |

### Service Status
- Status: ON for all / Organizational Units

### Google IdP Details for ITGlue
- SSO URL from Google Metadata
- Entity ID from Google Metadata
- Certificate download
```

## OneLogin

### OneLogin SSO Documentation
```markdown
## OneLogin SSO Configuration

### App Configuration
| Property | Value |
|----------|-------|
| App Name | ITGlue |
| SAML Signature Algorithm | SHA-256 |
| SAML nameID format | Email |

### ITGlue Subdomain
- Enter subdomain in app parameters

### User Assignment
- Assign roles or users
```

## Okta

### Okta SSO Documentation
```markdown
## Okta SSO Configuration

### SAML Settings
| Property | Value |
|----------|-------|
| Single Sign On URL | https://[subdomain].itglue.com/users/saml/auth |
| Audience URI | https://[subdomain].itglue.com/ |
| Name ID Format | EmailAddress |
| Application username | Email |

### Attribute Statements
| Name | Value |
|------|-------|
| email | user.email |

### Group Assignment
- Assign groups for access
```

## Just-in-Time Provisioning

### JIT Provisioning Documentation
```markdown
## Just-in-Time (JIT) Provisioning

### Description
Automatic user creation on first SSO login.

### Prerequisites
- SSO configured and active
- JIT enabled in ITGlue
- Email attribute in SAML token

### Configuration
| Setting | Value |
|---------|-------|
| JIT Provisioning | Enabled |
| Default Role | [Default Role] |
| Default Group | [Default Group] |

### Behavior
- New user created on SSO login
- Email taken from SAML assertion
- Default role assigned
- User gets immediate access
```

## SSO-Only Mode

### SSO-Only Documentation
```markdown
## SSO-Only Mode (Disable Password Login)

### Description
Enforces SSO-only login, disables password authentication.

### Prerequisites
- SSO fully tested
- All users SSO-capable
- Recovery admin identified

### Activation
1. Account > Authentication Settings
2. Enable "Enforce SSO"
3. Confirm

### Emergency Recovery
- Account Owner retains password login
- No lockout on IdP failure
```

## Multi-Factor Authentication

### MFA Documentation
```markdown
## Multi-Factor Authentication (MFA)

### Supported Methods
| Method | Provider |
|--------|----------|
| TOTP | Google Authenticator, Authy, etc. |
| Push | Duo Security |
| Hardware | YubiKey (via TOTP) |

### Enforcement
| Setting | Description |
|---------|-------------|
| Optional | User can enable MFA |
| Required | MFA enforced at login |

### Configuration
- Admin > Settings > Security
- Set MFA policy
- Grace period for setup
```

## SSO Troubleshooting

### Document SSO Errors
```markdown
## SSO Troubleshooting

### Common Errors
| Error | Cause | Solution |
|-------|-------|----------|
| Invalid Fingerprint | Certificate mismatch | Recalculate fingerprint |
| User Not Found | JIT disabled | Enable JIT or create user |
| Invalid ACS URL | URL mismatch | Verify ACS URL |
| Clock Skew | Time difference | Synchronize NTP |

### Debug Checklist
- [ ] Subdomain correct?
- [ ] Entity ID correct?
- [ ] ACS URL correct?
- [ ] Fingerprint current?
- [ ] Name ID Format = Email?
- [ ] Users assigned?
```

## Checklist SSO Configuration

### Preparation
- [ ] IdP admin access available
- [ ] ITGlue admin access available
- [ ] Subdomain identified

### IdP Configuration
- [ ] SAML app created
- [ ] ACS URL entered
- [ ] Entity ID entered
- [ ] Name ID = Email configured
- [ ] Users assigned

### ITGlue Configuration
- [ ] IdP SSO URL entered
- [ ] IdP Entity ID entered
- [ ] Fingerprint calculated and entered
- [ ] Fingerprint Algorithm = SHA256

### Test
- [ ] SSO login tested
- [ ] New user via JIT tested (if active)
- [ ] Logout/re-login tested

### Production
- [ ] SSO-Only mode enabled (optional)
- [ ] MFA policy defined
- [ ] Recovery process documented
