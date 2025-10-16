# Regel 2 - Datto RMM Skript-Richtlinien

## Zweck
Diese Regel erweitert die allgemeinen Skript-Richtlinien (Regel 5 von DevWing Core) um Datto RMM-spezifische Anforderungen.

## Datto RMM Input Variables

### Variablen-Verwendung

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

### Variablen-Typen

| Type | Beschreibung | Beispiel |
|------|--------------|----------|
| String | Freitext-Eingabe | Servicename, Benutzername |
| Selection | Dropdown-Liste | Vordefinierte Optionen |
| Boolean | "true" oder "false" (String!) | Feature aktivieren |
| Date | Datumsauswahl (String-Format) | Ziel-Datum |

**⚠️ Wichtig:** Booleans sind Strings – immer als String vergleichen!

### Variablen-Tabelle

Wenn Variablen genutzt werden, am Ende Tabelle ausgeben:

```markdown
🔧 Datto RMM Variables Configuration

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| serviceName | String | Spooler | Specifies which service should be restarted. |
| enableLogging | Boolean | true | Enables additional logging output. |
```

## Output-Struktur für Datto RMM

### Standard-Output
Bei normalen Scripts und Applications:
- Einfache `SUCCESS:` oder `ERROR:` Nachrichten
- Konsistente Struktur
- Keine sensiblen Daten

### Monitor-spezifische Struktur
Für Monitors gelten zusätzliche Anforderungen (siehe Regel 1 - Monitor).

## Post-Conditions

**Post-Conditions** sind Regeln, die nach Skript-Ausführung den Output (StdOut/StdErr) evaluieren.
Falls eine Regel zutrifft, endet der Job mit **Warning**-Status.

**Parameter:**
- **Warning Text**: Nachricht bei Trigger (case-sensitive)
- **Qualifier**: `Is found in` oder `Is not found in`
- **Resource**: `StdOut` oder `StdErr`

**Beispiel:**

| Setting | Value |
|---------|-------|
| Warning Text | ERROR: |
| Qualifier | Is found in |
| Resource | StdOut |

Wenn eine oder mehrere Post-Conditions erfüllt sind, endet der Job mit **Warning**, auch bei `exit 0`.

**Best Practice:** Nutze konsistente Error-Marker wie `ERROR:` oder `FAILED:` für automatische Warnungen.

## Beispiel: Datto RMM Service Restart (Linux)

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

### Datto RMM Variables Configuration für dieses Script

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| serviceName | String | nginx | Specifies which systemd service should be restarted. |

## Logo Prompt (Datto RMM Komponenten)

Nach erfolgreicher Erstellung **automatisch** Logo-Prompt ausgeben:

```
🎨 Logo Prompt:
Create a symbolic DevWing-style logo for "[Component Name]" —
[description].
Use clean flat iconography with subtle system symbolism, 
no text, and a blue-gray tech aesthetic.
```

**Logo-Regeln:**
- ❌ Kein Text
- ✅ Symbole, Formen, Farben
- ✅ Blau-Grau Tech-Ästhetik
- ✅ Themen: ⚙️ Automation, 💾 System, 🔄 Updates, 🔍 Analyse

## Component Description

Nach Logo Prompt kurze Beschreibung:

```
📝 Component Description:
Restarts a specified Linux service using systemctl.
```

**Regeln:**
- Max. 30 Wörter
- Beschreibt WAS, nicht WIE
- Plattform und Zweck erwähnen

## Hinweis zu Agent Variables

Siehe **Datto RMM Kontext** für Informationen zu vordefinierten Agent-Variablen wie:
- `CS_PROFILE_NAME`, `CS_ACCOUNT_UID`, etc.
- `UDF_1` bis `UDF_30`
