# Datto RMM - Platform Context

## Overview

**Datto RMM** (Remote Monitoring and Management) is a cloud-based RMM platform by **Kaseya**.
It enables IT service providers to centrally manage, monitor, and automate IT infrastructures.

## Supported Platforms

Datto RMM supports the following operating systems and scripting environments:

| Language | Target Agents | Notes |
|----------|--------------|-------|
| PowerShell | Windows | Recommended for all modern Windows devices |
| Batch (CMD) | Windows (Legacy) | For older Windows agents without PowerShell |
| VBScript | Windows | For special Windows automation and legacy systems |
| Shell (bash/sh) | Linux and macOS | For Unix-based agents |

### PowerShell Versions
- **Version 2.0**: Standard on Windows 7, available for Windows XP/Server 2003
- **Version 5.0**: Standard on Windows 10+
- **Compatibility**: Not all versions have the same feature set
- **Best Practice**: Ensure PowerShell 2.0 compatibility for broad OS support

**Setting minimum version:**
```powershell
#Requires -Version 3
```
This line must be the **first line** of the script. Older versions will reject the script without execution.

### PowerShell Signing (Self-Signed)
For self-signed PowerShell scripts:

**Prerequisite:** The certificate must be installed in the **Trusted Publishers Store** on the remote device.

**Process:**
1. Create self-signed certificate
2. Install certificate with tool like CertMgr in Trusted Publishers Certificate Store
3. Copy component script contents to local .ps1 file
4. Sign with tool like SignTool using the certificate
5. Copy signed contents back into the component script

### Shell/Bash (Unix/macOS)
**Shebang line required:**
```bash
#!/bin/bash
```

**Important:**
- This line **must** be at the start of every Unix script
- Selects the Command Interpreter (Bash is most compatible across *nix systems)
- macOS uses newer versions ZSH, but Bash remains compatible for scripts
- **Do not modify** unless you know exactly what you're doing!

### Comments in Scripts
- **PowerShell/Shell**: `# Comment`
- **Batch**: `REM Comment` or `:: Comment`
- **VBScript**: `' Comment`

### Batch Scripts Best Practices
**Recommendation:** Begin Batch scripts with `@echo off` for better output readability.

**Example:**
```batch
@echo off
set time=Morning
echo Good %time%
```

**Output:**
```
Good Morning
```

Without `@echo off`, each command itself would also be output, making the output cluttered.

## Component Categories

Datto RMM distinguishes three main categories:

### 1. Applications
- **Purpose**: Installation or execution of programs
- **Features**: Can contain files, use component credentials
- **Usage**: Software deployments, installations

### 2. Monitors
- **Purpose**: Monitoring system states
- **Behavior**: Regular execution for state checking
- **Alerts**: Can trigger alarms under certain conditions
- **⚠️ Important**: Category cannot be changed after creation!

### 3. Scripts
- **Purpose**: Execution of actions or automation
- **Behavior**: On-demand or scheduled execution
- **Features**: Can contain files, use component credentials
- **Usage**: Maintenance, updates, configuration

## Execution Context

### Permissions
- **Windows**: Runs as `NT AUTHORITY\SYSTEM` (LocalSystem Account)
- **Linux/macOS**: Runs as `root`
- Full system access – exercise caution!

### LocalSystem Account (Windows)
The LocalSystem Account has special characteristics:

**Advantages:**
- Extensive privileges on local endpoint
- Execution possible even when no one is logged on
- Works even with Limited User rights

**Limitations:**
- **No visible desktop**: Windows windows are hidden and non-interactive
  - Installers with "Next" buttons hang waiting for impossible input
- **No virtual keyboard/mouse**: No macro-based automation possible
- **No password**: No network authentication possible
- **No user resources**: No access to mapped network drives per user

**Alternative:**
Quick Jobs always run as LocalSystem. To execute scripts in the context of the logged-on user:
- Use Scheduled Job
- Utilize advanced options under "Execution"

### Runtime Environment
- Scripts run on remote agents
- No user interaction possible
- Stdout/Stderr are centrally captured
- Observe timeout limits (configurable per component)
- **LocalSystem Test**: Recommended to test scripts as LocalSystem to mimic Datto RMM behavior

### Execution Directory
Scripts are executed in a temporary **Package Directory** created by the Agent:
- Contains the script itself
- Contains all attached files (Attached Files)
- Contains simple metadata

**File Referencing:**
- Attached files can be referenced **without path specifications**
- For PowerShell: `file.txt` (direct)
- For Batch: `file.txt` (direct)
- For Shell: `file.txt` (direct)

## Component Level (Access Control)

Each component has a **Component Level** that controls access:

### Level Tiers
1. **Basic (1)** – Lowest tier
2. **Low (2)**
3. **Medium (3)**
4. **High (4)**
5. **Super (5)** – Highest tier

### Access Control
- Users only see components at or below their level
- Example: User with level "Medium (3)" only sees Basic (1), Low (2), and Medium (3)
- When editing, only available levels can be selected

## Attached Files

### Usage
- Only available for **Applications** and **Scripts**
- Multiple files possible
- **Max. file size**: 3 GB per file
- **Max. component size**: 5 GB total

### Important Notes
- File contents are **not** verified by Datto RMM
- Files can be referenced and used in scripts
- On upload failures, component is saved without affected files

## Component Credentials

### Purpose
Only available for **Applications** and **Scripts**.
Allows use of cached credentials that are unique per site.

### Usage
- Enable "Requires Component Credentials" checkbox
- Credentials are provided at runtime
- Useful for software installations or operations requiring authentication

## Site Restrictions

### Options
- **All Sites**: Component can be deployed to all sites
  - Requires access to all sites in the account
- **Selected Sites**: Restrict to selected sites only
  - Users without access to these sites won't see the component
  
**⚠️ Important**: Site restrictions only work for Quick Jobs and User Tasks!

## Agent Variables (Predefined Variables)

Datto RMM automatically provides several environment variables available in any supported scripting language:

### System Variables
- `CS_ACCOUNT_UID`: Unique ID of the Datto RMM account for this device
- `CS_CC_HOST`: Control Channel URI for Agent communication with Datto RMM
- `CS_CC_PORT1`: Port for Agent communication (HTTPS 443)
- `CS_CSM_ADDRESS`: Web interface address for this device
- `CS_DOMAIN`: Local device's domain (if connected)
- `CS_PROFILE_DESC`: Description of the site where this device is located
- `CS_PROFILE_NAME`: Name of the site where this device is located
- `CS_PROFILE_PROXY_TYPE`: 0 or 1 (proxy server configured?)
- `CS_PROFILE_UID`: Unique ID of the site for this device
- `CS_WS_ADDRESS`: Web Service URI for Agent communication with Datto RMM

### UDF Variables (User-Defined Fields)
- `UDF_1` through `UDF_30`: User-defined fields of the device
- **Important**: UDF data is only valid at the time of push
- On portal changes: Only available after Monitoring Policy refresh (at least once daily or manually)
- **Recommendation**: Use UDF data only for infrequently changing values in monitors

### Usage
**PowerShell:**
```powershell
$siteName = $env:CS_PROFILE_NAME
$customField = $env:UDF_1
```

**Batch/CMD:**
```batch
echo Site: %CS_PROFILE_NAME%
echo UDF: %UDF_1%
```

**Shell/Bash:**
```bash
echo "Site: $CS_PROFILE_NAME"
echo "UDF: $UDF_1"
```

## Input Variables

Datto RMM allows definition of **Input Variables**, which are injected at runtime as environment variables.

### Available Types
- **String/Value**: Free-text input
- **Selection**: Dropdown list with predefined options
- **Boolean**: "true" or "false" (as string!)
- **Date**: Date selection (string format)

### Usage in Scripts

**PowerShell:**
```powershell
$value = $env:variableName
```

**Batch/CMD:**
```batch
set "VALUE=%variableName%"
```

**Shell/Bash:**
```bash
VALUE="$variableName"
```

### Security Aspect
Input variables are used in scripts, but their values are hidden during local script inspection – useful for sensitive data.

**⚠️ Important:** Booleans are passed as strings ("true"/"false"), not as native booleans!

## Output Structure

### Result Block (Required for Monitors)
```
<-Start Result->
STATUS=Message
<-End Result->
```

### Diagnostic Block (Optional)
```
<-Start Diagnostic->
Additional diagnostic information
<-End Diagnostic->
```

**Rules:**
- `STATUS=` without space after `=`
- Markers must match exactly
- Diagnostics only for alerts (exit 1)
- Max. 60 seconds for diagnostic commands

## Exit Codes

- `exit 0` → **Success** (no alert)
- `exit 1` → **Failure** (alert triggered)

**Important:** Every component must end with one of these codes!

## Post-Conditions

Post-Conditions are rules that evaluate output after script execution.

### Parameters
- **Warning Text**: Text that triggers warning (case-sensitive)
- **Qualifier**: 
  - `Is found in` – Warning if text is found
  - `Is not found in` – Warning if text is not found
- **Resource**: `StdOut` or `StdErr`

### Behavior
When a post-condition is met, the job ends with **Warning** status, even with `exit 0`.

### Example
If `ERROR:` (case-sensitive) is defined as Warning Text:
- Datto RMM searches the script output
- If found: Job ends with **Warning** status
- Display: Orange "Warning" label in job results and Job Status column

**Best Practice:** Use consistent error markers like "ERROR:" or "FAILED:" for automatic warnings.

## Scope of Support (Datto RMM Support)

### What Datto Support covers:
✅ Troubleshooting jobs with scripts/components
✅ Investigation when script works locally but not in Datto RMM
✅ Platform-related issues with script execution

### What Datto Support does NOT cover:
❌ Creation of custom scripts/components
❌ Debugging custom script logic
❌ Script development or code review

**Alternative:** If assistance needed with script/component creation → Contact account manager for Professional Services engagement.

## Component Metadata

### Required Fields
- **Name**: Component name (required to save)
- **Description**: Brief description of the component
- **Category**: Applications, Monitors, or Scripts
- **Script Type**: PowerShell, Batch, Shell, or VBScript
- **Level**: Component Level (Basic to Super)

### Optional Fields
- **Icon**: 48x48 pixels (PNG/JPG/GIF)
  - PNG/JPG are automatically scaled
  - GIF is not scaled
  - Default icon used if none uploaded
- **Timeout**: Maximum execution time in seconds
- **Sites**: All or selected sites
- **Variables**: Input variables for runtime configuration
- **Files**: Attached files (only Applications/Scripts)
- **Post-Conditions**: Warning rules based on output

## Best Practices

### Security
- No sensitive data in logs
- Pass secrets via secure mechanisms
- Always validate input

### Performance
- Observe timeout limits
- Diagnostics under 60 seconds
- Use efficient commands

### Maintainability
- Clear, descriptive comments
- Structured outputs
- Consistent naming conventions

### Error Handling
- Check prerequisites
- Clean error messages
- Always end with correct exit code

## Platform URL

**Datto RMM** is a product by **Kaseya**.
More information: https://rmm.datto.com/help/en/Content/0HOME/Home.htm
