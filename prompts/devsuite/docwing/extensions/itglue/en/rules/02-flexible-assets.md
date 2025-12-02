# Rule 2 - Flexible Asset Templates

## Overview

Flexible Assets are custom documentation templates in ITGlue that store structured information in a repeatable format.

## Standard Templates

### Application Template
```yaml
Template Name: Application

Fields:
  - Name: Name
    Type: Text (Required)
    
  - Name: Manufacturer
    Type: Text
    
  - Name: Version
    Type: Text
    
  - Name: Description
    Type: Textbox
    
  - Name: Installation Type
    Type: Select
    Options: [On-Premise, Cloud, Hybrid]
    
  - Name: License Type
    Type: Select
    Options: [Subscription, Perpetual, Open Source, Freeware]
    
  - Name: License Key
    Type: Password (encrypted)
    
  - Name: Application Server
    Type: Tag (Configurations)
    
  - Name: Responsible Contact
    Type: Tag (Contacts)
    
  - Name: Support URL
    Type: URL
    
  - Name: Documentation
    Type: Textbox
    
  - Name: Notes
    Type: Textbox
```

### LAN/Network Template
```yaml
Template Name: LAN

Fields:
  - Name: Network Name
    Type: Text (Required)
    
  - Name: Network Type
    Type: Select
    Options: [LAN, WAN, WLAN, VPN, DMZ]
    
  - Name: Subnet
    Type: Text
    Example: 192.168.1.0/24
    
  - Name: Gateway
    Type: Text
    
  - Name: VLAN ID
    Type: Number
    
  - Name: DHCP Server
    Type: Tag (Configurations)
    
  - Name: DNS Server
    Type: Tag (Configurations)
    
  - Name: Firewall
    Type: Tag (Configurations)
    
  - Name: Switches
    Type: Tag (Configurations, Multiple)
    
  - Name: Description
    Type: Textbox
    
  - Name: Network Diagram
    Type: Upload
```

### Backup Template
```yaml
Template Name: Backup

Fields:
  - Name: Backup Name
    Type: Text (Required)
    
  - Name: Backup Solution
    Type: Select
    Options: [Datto SIRIS, Veeam, Acronis, Azure Backup, etc.]
    
  - Name: Backup Type
    Type: Select
    Options: [Full, Incremental, Differential, Continuous]
    
  - Name: Schedule
    Type: Text
    Example: Daily 10:00 PM, Weekly Sunday 02:00 AM
    
  - Name: Retention
    Type: Text
    Example: 30 days local, 1 year cloud
    
  - Name: Source Systems
    Type: Tag (Configurations, Multiple)
    
  - Name: Backup Target
    Type: Text
    
  - Name: Encryption
    Type: Checkbox
    
  - Name: Last Successful Test
    Type: Date
    
  - Name: Recovery Instructions
    Type: Textbox
    
  - Name: Credentials
    Type: Tag (Passwords)
```

## Template Creation Guidelines

### Field Types and Their Usage

| Field Type | Usage | Example |
|------------|-------|---------|
| **Text** | Short inputs | Name, Version |
| **Textbox** | Longer descriptions | Notes, Instructions |
| **Select** | Predefined options | Status, Type |
| **Number** | Numeric values | Port, VLAN ID |
| **Checkbox** | Yes/No fields | Active, Encrypted |
| **Date** | Date values | Expiration date, Last test |
| **URL** | Web links | Support URL, Documentation |
| **Password** | Sensitive data | License keys |
| **Tag** | Relationships | Servers, Contacts |
| **Upload** | File attachments | Diagrams, Certificates |

### Best Practices for Templates

1. **Use Required Fields Sparingly**
   - Only mark truly necessary fields as required
   - Minimum: Name field

2. **Use Grouping**
   - Group logically related fields together
   - Use headers/sections for clarity

3. **Add Descriptions**
   - Provide help text for each field
   - Include examples in help text

4. **Tags for Relationships**
   - Use tag fields for links to other assets
   - Allow multiple tags where appropriate

## Template Documentation

### Template README
Create documentation for each template:

```markdown
# [Template Name]

## Purpose
Description of what this template is used for.

## When to Use
- Situation 1
- Situation 2

## Required Fields
- Field 1: Description
- Field 2: Description

## Best Practices
- Recommendation 1
- Recommendation 2

## Example
[Completed example]
```

## Import/Export

### Template Export
- Templates can be exported as JSON
- Via Account → Export Data → Flexible Asset Templates

### Template Import
- Pre-built templates from ITGlue Template Library
- Import custom templates as JSON
- Account → Flexible Asset Types → Import
