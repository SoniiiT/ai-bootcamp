# Rule 6 - Component Naming

## Purpose
This rule defines how Datto RMM components should be named.

## Basic Principle

Component names should be **concise, descriptive, and professional**,
**without** including the words "Script" or "Monitor."

**Why?**
Datto RMM automatically identifies the component type – these terms are redundant.

## Naming Guidelines

### ✅ DO: Focus on Function

- **What** the component does – not **how** it's implemented
- Avoid redundant or technical suffixes: "Script", "Monitor", "Check", "Task"
- Use clear, action-oriented titles that describe purpose or target system

### ✅ DO: Use Title Case

- Capitalize each main word
- Example: `Windows 11 Upgrade`, not `windows 11 upgrade`

### ✅ DO: Keep it Short

- Ideal: **under 5 words**
- No unnecessary punctuation or version numbers in the title

## Examples

### ✅ Correct Names

| Component Name | Explanation |
|----------------|-------------|
| Windows 11 Upgrade | Performs or validates upgrade to Windows 11 |
| Windows 11 Driver Fix | Repairs or reinstalls drivers after upgrade |
| Hyper-V VM State | Checks and reports current virtual machine state |
| Disk Space Alert | Monitors available disk space and triggers alert if low |
| Service Restart Automation | Ensures critical services are running and restarts if stopped |

### ❌ Incorrect Names

| Incorrect | Problem |
|-----------|---------|
| Windows 11 Upgrade Script | Redundant – "Script" adds no value |
| Hyper-V VM State Monitor | Redundant – Datto RMM already knows it's a monitor |
| Disk Space Check Script | "Check" and "Script" both repeat the function type |
| Service Status Checker | "Checker" is redundant |
| File Monitoring Script | "Monitoring" and "Script" are unnecessary |

## Additional Notes

- Use **Title Case** (capitalize each main word)
- **No version numbers** in the title (e.g., not "v1.0")
- **No unnecessary punctuation** (e.g., no trailing hyphens)
- **Short and concise** – ideally under 5 words

## Scope

This rule applies exclusively to **Datto RMM Components** (Monitor and Component Naming) by Kaseya.

Different conventions may apply for other script types or platforms.
