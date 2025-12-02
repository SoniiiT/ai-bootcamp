# ITGlue Platform Context

## Platform Overview

ITGlue is Kaseya's leading IT documentation platform for Managed Service Providers (MSPs). The platform serves as the central knowledge base ("Single Source of Truth") for all IT-related documentation.

## Core Concepts

### Organization Structure
- **Organizations**: Main containers for customer data (corresponds to client companies)
- **Sub-Organizations**: Hierarchical sub-organizations for complex structures
- **Primary Organization**: The MSP's own organization

### Core Assets
| Asset Type | Description | Typical Contents |
|------------|-------------|------------------|
| **Configurations** | IT devices and systems | Servers, workstations, network devices, printers |
| **Contacts** | Contact persons | Name, email, phone, role, location |
| **Passwords** | Credentials | Username, password, URL, OTP, notes |
| **Locations** | Sites | Addresses, building information |
| **Domains** | Domains | Registrar, expiration date, DNS records |
| **SSL Certificates** | SSL certificates | Certificate data, expiration date |
| **Documents** | Documents | Free-text documentation, guides, SOPs |

### Flexible Assets
Custom documentation templates for structured information:
- **Applications**: Software and its configuration
- **LAN**: Network infrastructure
- **Backup**: Backup solutions and plans
- **Licensing**: License management
- **Custom Templates**: Freely creatable structures

### Special Features
- **Checklists/Runbooks**: Automated documentation workflows
- **Quick Notes**: Quick notes at organization level
- **Related Items**: Links between assets
- **Tags**: Categorization and filtering
- **Embedded Passwords**: Credentials embedded in assets
- **GlueFiles**: File attachments

## Network Glue
Automatic network documentation through agents:
- Active Directory integration
- Network topology mapping
- Device auto-discovery
- Configuration auto-matching
- Password rotation for AD accounts

## MyGlue
Customer portal for restricted access:
- Customers can manage their own passwords and documents
- Limited visibility based on permissions
- Chrome extension for autofill

## Integrations

### PSA Integrations
- **Autotask**: Bidirectional synchronization
- **ConnectWise Manage**: Organization and ticket sync
- **Kaseya BMS**: Native integration
- **Tigerpaw**: Configuration sync
- **Pulseway PSA**: Two-way sync

### RMM Integrations
- **Datto RMM**: Device and configuration sync
- **VSA 10**: Native Kaseya integration
- **Auvik**: Network device matching

### Microsoft Integrations
- **Microsoft 365**: User and license sync
- **Microsoft GDAP**: Tenant management
- **Azure AD/Entra ID**: Identity synchronization

## API & Automation
- RESTful API for programmatic access
- Webhook support for event notifications
- Export/import functions (CSV, JSON)

## Security Features
- **Vault**: Additional encryption layer for sensitive passwords
- **IP Access Control**: Access restriction by IP address
- **SSO/SAML**: Single Sign-On integration
- **MFA**: Multi-factor authentication
- **Audit Logs**: Complete activity logging
- **Revisions**: Version control for all assets

## Best Practices Terminology
- **SOPs** (Standard Operating Procedures): Standardized work instructions
- **Runbooks**: Automated checklists and workflows
- **Documentation Standards**: Consistent documentation guidelines
- **Relationship Mapping**: Linking related assets
