# Rule 2 - Datto RMM Script Guidelines

## Purpose
This rule extends the general script guidelines (Rule 5 from DevWing Core) with Datto RMM-specific requirements.

## Datto RMM Input Variables

### Variable Usage

**PowerShell:**
```powershell
$serviceName = $env:serviceName
```

**Batch/CMD:**
```batch
set "SERVICE=%serviceName%"
```

**Shell/Bash:**
```bash
SERVICE_NAME="$serviceName"
```

### Variable Types

| Type | Description | Example |
|------|-------------|---------|
| String | Free-text input | Service name, username |
| Selection | Dropdown list | Predefined options |
| Boolean | "true" or "false" (string!) | Enable feature |
| Date | Date selection (string format) | Target date |

**⚠️ Important:** Booleans are strings – always compare as strings!

### Variable Table

When variables are used, output table at the end:

```markdown
🔧 Datto RMM Variables Configuration

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| serviceName | String | Spooler | Specifies which service should be restarted. |
| enableLogging | Boolean | true | Enables additional logging output. |
```

## Output Structure for Datto RMM

### Standard Output
For regular Scripts and Applications:
- Simple `SUCCESS:` or `ERROR:` messages
- Consistent structure
- No sensitive data

### Monitor-specific Structure
For Monitors, additional requirements apply (see Rule 1 - Monitor).

## Post-Conditions

**Post-Conditions** are rules that evaluate script output (StdOut/StdErr) after execution.
If a rule matches, the job ends with **Warning** status.

**Parameters:**
- **Warning Text**: Message when triggered (case-sensitive)
- **Qualifier**: `Is found in` or `Is not found in`
- **Resource**: `StdOut` or `StdErr`

**Example:**

| Setting | Value |
|---------|-------|
| Warning Text | ERROR: |
| Qualifier | Is found in |
| Resource | StdOut |

If one or more post-conditions are met, the job ends with **Warning**, even with `exit 0`.

**Best Practice:** Use consistent error markers like `ERROR:` or `FAILED:` for automatic warnings.

## Example: Datto RMM Service Restart (Linux)

```bash
#!/bin/bash
# *************************************************************************************
# Component: Service Restart [LIN] - DevWing Version
# Author: <Author Name>
# Purpose: Restarts a given service defined via Datto RMM input variable
# Version: 1.0
# *************************************************************************************

SERVICE_NAME="$serviceName"

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

### Datto RMM Variables Configuration for this Script

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| serviceName | String | nginx | Specifies which systemd service should be restarted. |

## Logo Prompt (Datto RMM Components)

After successful creation, **automatically** output logo prompt:

```
🎨 Logo Prompt:
Create a symbolic DevWing-style logo for "[Component Name]" —
[description].
Use clean flat iconography with subtle system symbolism, 
no text, and a blue-gray tech aesthetic.
```

**Logo Rules:**
- ❌ No text
- ✅ Symbols, shapes, colors
- ✅ Blue-gray tech aesthetic
- ✅ Themes: ⚙️ Automation, 💾 System, 🔄 Updates, 🔍 Analysis

## Component Description

After logo prompt, brief description:

```
📝 Component Description:
Restarts a specified Linux service using systemctl.
```

**Rules:**
- Max. 30 words
- Describes WHAT, not HOW
- Mention platform and purpose

## Note on Agent Variables

See **Datto RMM Context** for information on predefined Agent variables like:
- `CS_PROFILE_NAME`, `CS_ACCOUNT_UID`, etc.
- `UDF_1` through `UDF_30`
