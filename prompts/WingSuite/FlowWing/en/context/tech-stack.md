# Workflow Automation Tech Stack

## Overview
This context defines the general technologies and concepts FlowWing works with. It serves as a baseline for automation tasks before specific platform extensions are applied.

## Core Concepts

### Triggers
- **Webhook**: Real-time data push via HTTP POST.
- **Polling**: Checking for changes at regular intervals.
- **Schedule**: Time-based execution (Cron).
- **App Event**: Specific event in a connected app (e.g., "New Email").

### Actions
- **CRUD Operations**: Create, Read, Update, Delete data.
- **Transformation**: Modifying data structure (JSON parse, String manipulation).
- **Logic**: If/Else, Switch, Wait, Terminate.

### Data Formats
- **JSON**: The standard for most modern APIs and workflow engines (n8n, Logic Apps).
- **YAML**: Often used for definition-as-code (Kestra, GitHub Actions).
- **Expressions**: Platform-specific formulas (Power Automate Expressions, Excel-like formulas).

## Supported Platforms (General Knowledge)

### Low-Code / No-Code
- **Microsoft Power Automate**: Enterprise focus, Microsoft 365 integration.
- **Zapier**: Consumer/SMB focus, vast connector library.
- **Make (formerly Integromat)**: Visual, complex data scenarios.

### Developer / Self-Hosted
- **n8n**: Node-based, visual but code-friendly (JavaScript).
- **Kestra**: Orchestration-as-Code, Java/YAML based.
- **Node-RED**: IoT and event-driven, flow-based programming.

## Best Practices

1. **Idempotency**: Ensure flows can run multiple times without creating duplicates.
2. **Error Handling**: Always implement "On Error" paths or Try/Catch scopes.
3. **Environment Variables**: Use variables for configuration (URLs, emails) instead of hardcoding.
