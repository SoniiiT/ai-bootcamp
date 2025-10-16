# Rule 4 - Datto RMM Monitor Components

## Purpose
This rule defines the structure and conventions for Datto RMM monitor component scripts.

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
- `[WIN/LIN/MAC]` → Cross-platform (all OS)

**Versioning:**
- Line always ends with "- DevWing Version"

## Exit Codes

```
exit 0 → success (no alert)
exit 1 → failure (alert)
```

Every script **must** end with one of these exit codes.

## Required Output Blocks

### Result Block (Mandatory)

```
<-Start Result->
STATUS=Your message
<-End Result->
```

**Rules:**
- `STATUS=` without space after `=`
- Message in one line
- Markers must match exactly

### Diagnostic Block (Optional, only for alerts)

```
<-Start Diagnostic->
Additional information or diagnostic output
<-End Diagnostic->
```

**When to use:**
- Only with `exit 1` (error/alert)
- Output before `exit`
- Max. 60 seconds runtime

## Runtime Context

- **Windows**: Runs as `NT AUTHORITY\SYSTEM`
- **Linux/macOS**: Runs as `root`
- Local tests may differ due to permissions

## Datto RMM Input Variables

### Variable Usage

**PowerShell:**
```powershell
$path = $env:filePath
```

**Batch/CMD:**
```batch
set "FILE=%filePath%"
```

**Shell/Bash:**
```bash
FILE_PATH="$filePath"
```

### Variable Types

| Type | Description | Example |
|------|-------------|---------|
| String | Free-text input | File path, service name |
| Selection | Dropdown list | Select from options |
| Boolean | "true" or "false" (as string!) | Enable diagnostics |
| Date | Date selection (string format) | Execution date |

**⚠️ Important:** Booleans are strings! Always compare as strings:
```powershell
if ($env:diagEnabled -eq "true") { ... }
```

### Variable Table Output

When a script uses variables, output a table at the end:

```markdown
🔧 Datto RMM Variables Configuration

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| filePath | String | C:\Test\test.txt | Absolute path to the file to check. |
| diagEnabled | Boolean | true | If true, print diagnostic block on failure. |
```

## Example: PowerShell Monitor with Variables

```powershell
# *************************************************************************************
# Component: File Presence Monitor [WIN] - DevWing Version
# Author: <Author Name>
# Purpose: Checks whether a configurable file exists; optional diagnostics on failure
# Version: 1.0
# *************************************************************************************

$path = $env:filePath
$diag = $env:diagEnabled

if (Test-Path $path -ErrorAction SilentlyContinue) {
    Write-Host '<-Start Result->'
    Write-Host "STATUS=File present: $path"
    Write-Host '<-End Result->'
    exit 0
}
else {
    Write-Host '<-Start Result->'
    Write-Host "STATUS=File absent: $path"
    Write-Host '<-End Result->'

    if ($diag -eq "true") {
        Write-Host '<-Start Diagnostic->'
        Write-Host "- Programs running in memory at alert time:"
        Get-Process
        Write-Host '<-End Diagnostic->'
    }

    exit 1
}
```

## Testing Mode

### Phase 1: Test Version
First output only functional test version:
- No header
- No formatting
- Only core logic
- Quickly testable

### Phase 2: Production Version
After confirmation ("It works!"):
1. Ask: "Would you like me to apply the DevWing rules and create the full Datto RMM component?"
2. After confirmation: Full version with header, exit codes, description, and logo prompt

## Logo Prompt Rule

After successful script creation, **automatically** generate a logo prompt:

```
🎨 Logo Prompt:
Create a symbolic DevWing-style logo for "[Component Name]" —
[description of what it does].
Use clean flat iconography with subtle system symbolism, 
no text, and a blue-gray tech aesthetic.
```

**Logo Rules:**
- ❌ No text in the logo
- ✅ Only symbols, shapes, and colors
- ✅ Blue-gray tech aesthetic
- ✅ Flat, modern iconography
- ✅ Themes: ⚙️ Automation, 💾 System, 🔄 Restart, 🔍 Monitoring

## Component Description

After the logo prompt, output a brief description:

```
📝 Component Description:
Monitors and restarts a specified Linux or macOS service using systemctl if inactive.
```

**Rules:**
- Max. 30 words (1-3 sentences)
- Describes WHAT, not HOW
- Mention platform and purpose
- Clear, neutral English
