# Rule 09: Browser Extension and Password Features

## Purpose
This rule defines documentation standards for the ITGlue Browser Extension, Autofill, OTP, and SafeShare features.

## Browser Extension Documentation

### Extension Overview
```markdown
## ITGlue Browser Extension

### Supported Browsers
| Browser | Version | Manifest |
|---------|---------|----------|
| Chrome | Current | V3 |
| Firefox | Current | V3 |
| Edge | Via Chrome Web Store | V3 |

### Features
- Password search
- Autofill for credentials
- OTP generation
- Quick-Search Hotkey
- Offline mode (optional)
```

### Extension Settings
```markdown
## Extension Configuration

### General Settings
| Setting | Value | Description |
|---------|-------|-------------|
| Subdomain | [subdomain] | ITGlue Account URL |
| Auto-logout | [Minutes] | Inactivity timeout |
| Quick-Search Hotkey | [Key combination] | Open quick search |

### Security Settings
- Biometric authentication: Yes/No
- Password caching: [Duration]
- Automatic lock: [Minutes]
```

## Autofill Documentation

### Autofill Setup
```markdown
## Autofill Configuration

### Prerequisites
- Browser Extension installed
- User logged into ITGlue
- Password saved with URL

### How It Works
1. Open website with saved password
2. Extension automatically recognizes URL
3. Credentials are suggested
4. Insert with click or hotkey

### Best Practices
- URL exactly as saved in password
- Multiple URLs per password possible
- Wildcard matching for subdomains
```

### URL Matching Documentation
```markdown
## URL Matching for Autofill

### Matching Rules
| URL Type | Example | Matching |
|----------|---------|----------|
| Exact | https://portal.example.com | Only this URL |
| Domain | example.com | All subdomains |
| Path | example.com/admin | Only with path |

### Tips
- Prefer HTTPS
- Include port if non-standard
- Multiple URLs for different access points
```

## OTP (One-Time Password)

### OTP Documentation
```markdown
## OTP Configuration

### Storing OTP Secret Key
| Field | Description |
|-------|-------------|
| OTP Secret Key | Base32-encoded key |
| Issuer | Service name |
| Account | Username/Email |

### OTP Usage
1. Open password in extension
2. OTP code is automatically generated
3. Code is valid for 30 seconds
4. Automatic copy available

### Export Note
- OTP Secret Key is included in CSV exports
- OTP import is NOT supported
```

### OTP in List View
```markdown
## OTP Quick Access

### List View Feature
- Copy OTP code directly from password list
- No need to open password details
- Shows remaining validity time
```

## Password SafeShare

### SafeShare Documentation
```markdown
## Password SafeShare

### Feature Description
Securely share passwords with external people who do NOT have an ITGlue account.

### Prerequisites
- SafeShare enabled at account level
- SafeShare enabled for individual passwords
- Valid email address of recipient

### Restrictions
- Vaulted passwords CANNOT be shared
- Expiration date required
- Maximum access count configurable

### SafeShare Configuration
| Setting | Value | Description |
|---------|-------|-------------|
| Expiration | [Hours/Days] | Automatic deactivation |
| Max. Accesses | [Count] | Limit for views |
| Notification | Yes/No | Email on access |
```

### SafeShare Documentation Template
```markdown
## SafeShare Log: [Password Name]

### Shared Information
| Date | Recipient | Expiration | Status |
|------|-----------|------------|--------|
| [Date] | [Email] | [Date] | Active/Expired |

### Access Log
| Timestamp | IP Address | Status |
|-----------|------------|--------|
| [Date/Time] | [IP] | Successful |
```

## Password Categories

### Category Documentation
```markdown
## Password Categorization

### Standard Categories
| Category | Usage |
|----------|-------|
| Credentials | General login credentials |
| Domain Admin | Domain administrator |
| Server | Server access |
| Network | Network devices |
| Cloud | Cloud services |
| Vendor | Vendor portals |

### Custom Categories
| Category | Description | Created On |
|----------|-------------|------------|
| [Name] | [Description] | [Date] |

### Category Guidelines
- Use consistent naming
- Don't create too many categories
- Perform regular cleanup
```

## Offline Mode

### Offline Extension Documentation
```markdown
## Offline Mode Configuration

### Activation
- Admin > Settings > Offline Mode
- Enable "Enable Offline Mode Extension"

### Security Settings
| Setting | Value | Description |
|---------|-------|-------------|
| Auto-Wipe | [Days] | Delete data after X days offline |
| Wipe Warning | [Days] | Send email warning |
| Inactivity Logout | [Minutes] | Automatic logout |

### SSO in Offline Mode
- SSO users can use offline mode
- Separate authentication required
- Configuration under Admin > Settings > Offline Mode
```

## Mobile App

### Mobile Features Documentation
```markdown
## ITGlue Mobile App

### Features
- Password access
- OTP generation
- Password generator
- Biometric authentication
- QR/Barcode scanning

### Platforms
| Platform | Version |
|----------|---------|
| iOS | [Version] |
| Android | [Version] |

### Security
- Biometrics: Touch ID / Face ID / Fingerprint
- PIN fallback available
- Data stored encrypted
```

## Browser Extension Checklist

- [ ] Extension installed and current
- [ ] Subdomain configured
- [ ] Hotkeys set up
- [ ] Autofill tested
- [ ] OTP functionality verified
- [ ] Offline mode enabled (if needed)
- [ ] Mobile app set up (optional)
- [ ] Biometric authentication configured
