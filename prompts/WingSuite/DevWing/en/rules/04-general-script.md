# Rule 5 - General Script Guidelines

## Purpose
This rule defines best practices for script creation across different platforms.

## Mandatory Header

Every script must start with the following header:

```
# *************************************************************************************
# Component: <Component Name> [WIN/LIN/MAC] - DevWing Version
# Author: <Author Name>
# Purpose: <Short description of what the script does>
# Version: 1.0
# *************************************************************************************
```

### Header Rules

**Platform Tags:**
- `[WIN]` → Windows only
- `[LIN]` → Linux only
- `[MAC]` → macOS only
- `[LIN/MAC]` → Linux and macOS
- `[WIN/LIN/MAC]` → Cross-platform

**Versioning:**
- Line ends with "- DevWing Version"

## Core Requirements

### 1. Exit Codes

```
exit 0 → success
exit 1 → failure, warning, or alert state
```

**Always** end with one of these codes – even with handled exceptions.

### 2. Runtime Awareness

Scripts must detect and respect their context:

**To check:**
- **OS**: Windows / Linux / macOS
- **Interpreter**: PowerShell, bash, Python, Node, CMD, etc.
- **Privilege Level**: root / SYSTEM / admin / user
- **Architecture**: x86 / x64 / ARM

**For unsupported environments:**
- Output clear error message
- Exit with `exit 1`

Example:
```
ERROR: Unsupported runtime - requires Linux bash, detected Windows PowerShell.
```

### 3. Input & Configuration

- Use parameters, environment variables, or config files
- **No interactive user input** (unless explicitly requested)
- Provide defaults where possible
- Validate all external inputs – `exit 1` on invalid data

### 4. Error Handling

- Check prerequisites (files, permissions, dependencies)
- Use `try/catch` or equivalent
- Output concise error reason to stdout/stderr
- Exit with `exit 1`
- No unhandled exceptions or silent failures

### 5. Output & Logging

- Keep output clean and one message per line
- Use stderr for errors (if supported)
- **No sensitive data** in logs (passwords, tokens, keys)
- Prefer simple, human-readable messages:
  - `SUCCESS: Operation completed successfully`
  - `ERROR: File not found`
- Consistent structure for automation parsing

### 6. Security Practices

- Quote or escape variables (prevent injection)
- Use least privilege where possible
- Verify downloaded or executed external content
- **No plain-text passwords**

### 7. Idempotence & Re-Execution

- Scripts should be safe to run multiple times
- If not idempotent: clearly comment side effects

### 8. Documentation

- Comment every logical section (Why, not just What)
- List prerequisites or dependencies at the top (if relevant)

## Runtime Context Expectations

Where relevant, scripts should detect and adapt to:

| Context | Example |
|---------|---------|
| OS | Windows, Linux, macOS |
| Interpreter | bash, pwsh, python, node, cmd.exe |
| Privilege | root, NT AUTHORITY\SYSTEM, Administrator, user |
| Architecture | x64, x86, arm64 |
| Tool versions | `python --version`, `pwsh -v`, `node -v` |

**Unsupported environments:**
- Output clear message
- Exit with `exit 1`

## Example: Service Restart (Linux)

```bash
#!/bin/bash
# *************************************************************************************
# Component: Service Restart [LIN] - DevWing Version
# Author: <Author Name>
# Purpose: Restarts a specified systemd service
# Version: 1.0
# *************************************************************************************

SERVICE_NAME="${1:-nginx}"

if [ -z "$SERVICE_NAME" ]; then
  echo "ERROR: No service name provided."
  exit 1
fi

systemctl restart "$SERVICE_NAME" 2>/dev/null
if [ $? -eq 0 ]; then
  echo "SUCCESS: Service $SERVICE_NAME restarted successfully."
  exit 0
else
  echo "ERROR: Failed to restart $SERVICE_NAME."
  exit 1
fi
```

## Testing Mode

### Phase 1: Test Version
First output only functional test version:
- No header, no formatting
- Only core logic
- Quickly testable

### Phase 2: Production Version
After confirmation ("It works!"):
1. Ask: "Would you like me to apply the DevWing rules?"
2. After confirmation: Full version with header, exit codes, documentation
