# Regel 4 - Datto RMM Monitor-Komponenten

## Zweck
Diese Regel definiert die Struktur und Konventionen für Datto RMM Monitor-Komponenten-Skripte.

## Pflicht-Header

Jedes Skript muss mit folgendem Header beginnen:

```
# *************************************************************************************
# Component: <Component Name> [WIN/LIN/MAC]
# Author: SoniiiT
# Purpose: <Short description of what the script does>
# Version: 1.0
# *************************************************************************************
```

### Header-Regeln

**Plattform-Tags:**
- `[WIN]` → Nur Windows
- `[LIN]` → Nur Linux
- `[MAC]` → Nur macOS
- `[LIN/MAC]` → Linux und macOS
- `[WIN/LIN/MAC]` → Cross-Platform (alle OS)

## Exit-Codes

```
exit 0 → Erfolg (kein Alert)
exit 1 → Fehler (Alert)
```

Jedes Skript **muss** mit einem dieser Exit-Codes enden.

## Pflicht-Output-Blöcke

### Result-Block (Pflicht)

```
<-Start Result->
STATUS=Your message
<-End Result->
```

**Regeln:**
- `STATUS=` ohne Leerzeichen nach `=`
- Nachricht in einer Zeile
- Marker müssen exakt übereinstimmen

### Diagnostic-Block (Optional, nur bei Alerts)

```
<-Start Diagnostic->
Additional information or diagnostic output
<-End Diagnostic->
```

**Wann verwenden:**
- Nur bei `exit 1` (Fehler/Alert)
- Vor dem `exit` ausgeben
- Max. 60 Sekunden Laufzeit

## Runtime-Kontext

- **Windows**: Läuft als `NT AUTHORITY\SYSTEM`
- **Linux/macOS**: Läuft als `root`
- Lokale Tests können aufgrund von Berechtigungen abweichen

## Datto RMM Input Variables

### Variablen-Verwendung

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

### Variablen-Typen

| Type | Beschreibung | Beispiel |
|------|--------------|----------|
| String | Freitext-Eingabe | Dateipfad, Servicename |
| Selection | Dropdown-Liste | Auswahl aus Optionen |
| Boolean | "true" oder "false" (als String!) | Diagnose aktivieren |
| Date | Datum-Auswahl (String-Format) | Ausführungsdatum |

**⚠️ Wichtig:** Booleans sind Strings! Immer als String vergleichen:
```powershell
if ($env:diagEnabled -eq "true") { ... }
```

### Variablen-Tabelle ausgeben

Wenn ein Skript Variablen nutzt, muss am Ende eine Tabelle ausgegeben werden:

```markdown
🔧 Datto RMM Variables Configuration

| Name | Type | Default value | Description |
|------|------|---------------|-------------|
| filePath | String | C:\Test\test.txt | Absolute path to the file to check. |
| diagEnabled | Boolean | true | If true, print diagnostic block on failure. |
```

## Beispiel: PowerShell Monitor mit Variablen

```powershell
# *************************************************************************************
# Component: File Presence Monitor [WIN]
# Author: SoniiiT
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

### Phase 1: Test-Version
Zuerst nur funktionale Test-Version ausgeben:
- Kein Header
- Keine Formatierung
- Nur Core-Logik
- Schnell testbar

### Phase 2: Produktions-Version
Nach Bestätigung ("Funktioniert!"):
1. Fragen: "Soll ich die DevWing-Regeln anwenden und die vollständige Datto RMM-Komponente erstellen?"
2. Nach Bestätigung: Full Version mit Header, Exit-Codes, Description und Logo Prompt

## Logo Prompt Regel

Nach erfolgreicher Skript-Erstellung **automatisch** einen Logo-Prompt generieren:

```
🎨 Logo Prompt:
Create a symbolic DevWing-style logo for "[Component Name]" —
[description of what it does].
Use clean flat iconography with subtle system symbolism, 
no text, and a blue-gray tech aesthetic.
```

**Logo-Regeln:**
- ❌ Kein Text im Logo
- ✅ Nur Symbole, Formen und Farben
- ✅ Blau-Grau Tech-Ästhetik
- ✅ Flache, moderne Iconografie
- ✅ Themen: ⚙️ Automation, 💾 System, 🔄 Restart, 🔍 Monitoring

## Component Description

Nach dem Logo Prompt eine kurze Beschreibung ausgeben:

```
📝 Component Description:
Monitors and restarts a specified Linux or macOS service using systemctl if inactive.
```

**Regeln:**
- Max. 30 Wörter (1-3 Sätze)
- Beschreibt WAS, nicht WIE
- Plattform und Zweck erwähnen
- Klares, neutrales Englisch
