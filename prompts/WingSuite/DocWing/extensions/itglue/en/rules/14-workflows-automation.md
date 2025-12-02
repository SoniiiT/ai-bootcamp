# Rule 14: Workflows & Automation

## Purpose
This rule defines standards for documenting workflows, automations, and notifications in ITGlue.

## Flags (Labels)

### Flag Documentation
```markdown
## ITGlue Flags: Overview

### Available Flags
| Flag | Color | Usage |
|------|-------|-------|
| Needs Review | Orange | Document needs review |
| Reviewed | Green | Document has been reviewed |
| Outdated | Red | Document is outdated |
| Important | Yellow | Important document |
| Work in Progress | Blue | Document in progress |

### Flag Workflow
1. Create document → "Work in Progress"
2. Complete document → "Needs Review"
3. Conduct review → "Reviewed"
4. Identify outdated → "Outdated"
```

### Document Flag Rules
```markdown
## Flag Guidelines

### Assignment
| Situation | Flag |
|-----------|------|
| New document | Work in Progress |
| Ready for review | Needs Review |
| After review | Reviewed |
| Older than 90 days | Needs Review |
| No longer current | Outdated |

### Responsibilities
| Role | Actions |
|------|---------|
| Author | Set flag to "Needs Review" |
| Reviewer | Set flag to "Reviewed" |
| Admin | Review regular flag reports |
```

## Webhooks

### Webhook Documentation
```markdown
## ITGlue Webhooks: Configuration

### Webhook Overview
| Property | Value |
|----------|-------|
| Endpoint URL | [Webhook URL] |
| Active | Yes/No |
| Events | [Event Types] |

### Supported Events
| Event | Description |
|-------|-------------|
| password.created | Password created |
| password.updated | Password changed |
| password.deleted | Password deleted |
| document.created | Document created |
| document.updated | Document changed |
| configuration.created | Configuration created |
| organization.created | Organization created |
```

### Webhook Payload Example
```markdown
## Webhook Payload Structure

### Example: password.updated
```json
{
  "event": "password.updated",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "id": 12345,
    "name": "Admin Password",
    "organization_id": 100,
    "resource_type": "Password",
    "updated_by": "user@example.com"
  }
}
```

### Processing
- Webhook receiver must return HTTP 200
- Timeout: 30 seconds
- Retry on error: 3 attempts
```

## Email Notifications

### Notification Settings
```markdown
## ITGlue Email Notifications

### User Settings
| Notification | Default | Description |
|--------------|---------|-------------|
| Password Expiry | On | X days before expiry |
| Document Updates | Off | On changes |
| Review Reminders | On | For assigned reviews |
| Weekly Digest | Off | Weekly summary |

### Configuration
- User > Preferences > Notifications
- Administrator can set defaults
```

### Password Expiry Notifications
```markdown
## Password Expiry Notifications

### Timeframes
| Days Before Expiry | Action |
|--------------------|--------|
| 30 days | First warning |
| 14 days | Reminder |
| 7 days | Urgent warning |
| 0 days | Expired notification |

### Recipients
- Password creator
- Assigned users
- Admin (optional)
```

## Slack Integration

### Document Slack Notifications
```markdown
## ITGlue Slack Integration

### Configuration
| Property | Value |
|----------|-------|
| Workspace | [Slack Workspace] |
| Channel | [Slack Channel] |
| Bot Name | ITGlue |
| Events | [Configured Events] |

### Setup
1. Create/install Slack app
2. Generate webhook URL
3. Enter in ITGlue under Admin > Integrations > Slack
4. Select events

### Supported Events
- Password created/changed
- Document created/changed
- Configuration changed
- Review required
```

## Microsoft Teams Integration

### Document Teams Notifications
```markdown
## ITGlue Microsoft Teams Integration

### Configuration
| Property | Value |
|----------|-------|
| Team | [Team Name] |
| Channel | [Channel Name] |
| Webhook URL | [Incoming Webhook URL] |

### Setup via Workflows
1. Teams > Channel > Workflows
2. "Post to a channel when a webhook request is received"
3. Copy workflow URL
4. Configure in ITGlue webhook

### Alternative: Power Automate
- Receive webhook
- Post message to Teams
- Adaptive Cards for richer display
```

## Zapier Integration

### Document Zapier Workflows
```markdown
## ITGlue Zapier Integration

### Triggers (ITGlue → Other Apps)
| Trigger | Description |
|---------|-------------|
| New Password | New password created |
| Updated Password | Password changed |
| New Document | New document |
| New Configuration | New configuration |

### Actions (Other Apps → ITGlue)
| Action | Description |
|--------|-------------|
| Create Password | Create password |
| Create Document | Create document |
| Create Configuration | Create configuration |

### Example Zaps
| Zap | Trigger | Action |
|-----|---------|--------|
| Ticket → ITGlue | New ConnectWise Ticket | Create ITGlue Document |
| Password Alert | ITGlue Password Expiring | Slack Message |
| New Client Setup | New Organization | Create Trello Card |
```

## Automated Workflows

### Workflow Documentation Template
```markdown
## Automated Workflow: [Name]

### Overview
| Property | Value |
|----------|-------|
| Trigger | [Trigger] |
| Target | [Target Action] |
| Frequency | [One-time/Recurring] |
| Status | Active/Inactive |

### Workflow Steps
1. [Trigger description]
2. [Check conditions]
3. [Execute action]
4. [Send notification]

### Involved Systems
| System | Role |
|--------|------|
| ITGlue | [Trigger/Action] |
| [System 2] | [Role] |

### Error Handling
- On error: [Action]
- Escalate to: [Person/Team]
```

## Review Workflows

### Document Review Process
```markdown
## Document Review Workflow

### Process
1. Author sets flag to "Needs Review"
2. Reviewer receives notification
3. Reviewer checks document
4. Reviewer sets flag to "Reviewed"
5. Author receives confirmation

### Automatic Review Triggers
| Trigger | Action |
|---------|--------|
| 90 days without update | Flag to "Needs Review" |
| Expiry date reached | Notification to author |
| Critical update | Immediate review |

### Review Checklist
- [ ] Content current?
- [ ] Links working?
- [ ] Related Items correct?
- [ ] Tags appropriate?
```

## Audit Trail Usage

### Document Audit Trail
```markdown
## ITGlue Audit Trail

### Trackable Actions
| Action | Details |
|--------|---------|
| Login | User, IP, timestamp |
| Password View | Who accessed when |
| Document Edit | Changes, user |
| Permission Change | Before/After |

### Audit Reports
- Admin > Reports > Audit Log
- Filter by time period, user, action
- Export as CSV

### Compliance Usage
- Evidence for audits
- Validate access control
- Track changes
```

## Automation Best Practices

### Best Practices
```markdown
## Automation: Best Practices

### Webhook Security
- Use HTTPS endpoints
- Validate webhook secrets
- Implement rate limiting

### Notification Hygiene
- Don't over-notify
- Select relevant events
- Avoid duplicates

### Workflow Documentation
- Document every workflow
- Name responsible parties
- Regular review

### Monitoring
- Monitor webhook errors
- Check notification delivery
- Review audit log regularly
```

## Automation Checklist

### Webhooks
- [ ] Webhook endpoint defined
- [ ] HTTPS enabled
- [ ] Events selected
- [ ] Test data sent
- [ ] Error handling implemented

### Notifications
- [ ] Email notifications configured
- [ ] Slack/Teams integrated (optional)
- [ ] Password expiry warnings active
- [ ] Review reminders active

### Workflows
- [ ] Workflows documented
- [ ] Responsible parties named
- [ ] Test run completed
- [ ] Monitoring active
