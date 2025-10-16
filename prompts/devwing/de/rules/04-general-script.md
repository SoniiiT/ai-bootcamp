# Regel 5 - Allgemeine Skript-Richtlinien

## Zweck
Diese Regel definiert Best Practices für Skript-Erstellung über verschiedene Plattformen hinweg.

## Pflicht-Header

Jedes Skript muss mit folgendem Header beginnen:

```
# *************************************************************************************
# Component: <Component Name> [WIN/LIN/MAC] - DevWing Version
# Author: <Author Name>
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
- `[WIN/LIN/MAC]` → Cross-Platform

**Versionierung:**
- Zeile endet mit "- DevWing Version"

## Kern-Anforderungen

### 1. Exit-Codes

```
exit 0 → Erfolg
exit 1 → Fehler, Warnung oder Alert-Zustand
```

**Immer** mit einem dieser Codes beenden – auch bei behandelten Exceptions.

### 2. Runtime-Awareness

Skripte müssen ihren Kontext erkennen und respektieren:

**Zu prüfen:**
- **OS**: Windows / Linux / macOS
- **Interpreter**: PowerShell, bash, Python, Node, CMD, etc.
- **Privilege Level**: root / SYSTEM / admin / user
- **Architektur**: x86 / x64 / ARM

**Bei nicht unterstützter Umgebung:**
- Klare Fehlermeldung ausgeben
- Mit `exit 1` beenden

Beispiel:
```
ERROR: Unsupported runtime - requires Linux bash, detected Windows PowerShell.
```

### 3. Input & Konfiguration

- Parameter, Umgebungsvariablen oder Config-Files nutzen
- **Keine interaktive Benutzereingabe** (außer explizit gewünscht)
- Defaults bereitstellen, wo möglich
- Alle externen Inputs validieren – bei ungültigen Daten `exit 1`

### 4. Fehlerbehandlung

- Prerequisites prüfen (Dateien, Berechtigungen, Dependencies)
- `try/catch` oder Äquivalent nutzen
- Prägnanten Fehlergrund auf stdout/stderr ausgeben
- Mit `exit 1` beenden
- Keine unbehandelten Exceptions oder stille Fehler

### 5. Output & Logging

- Output sauber und einzeilig pro Nachricht
- stderr für Fehler nutzen (wenn unterstützt)
- **Keine sensiblen Daten** loggen (Passwörter, Tokens, Keys)
- Einfache, menschenlesbare Nachrichten bevorzugen:
  - `SUCCESS: Operation completed successfully`
  - `ERROR: File not found`
- Konsistente Struktur für Automatisierungs-Parsing

### 6. Sicherheitspraktiken

- Variablen quoten oder escapen (Injection-Prävention)
- Least Privilege wo möglich
- Heruntergeladene oder ausgeführte externe Inhalte verifizieren
- **Keine Klartextpasswörter** speichern

### 7. Idempotenz & Wiederausführung

- Skripte sollten sicher mehrfach ausführbar sein
- Falls nicht idempotent: Seiteneffekte klar kommentieren

### 8. Dokumentation

- Jeden logischen Abschnitt kommentieren (Warum, nicht nur Was)
- Prerequisites oder Dependencies am Anfang auflisten (falls relevant)

## Runtime-Kontext-Erwartungen

Wo relevant, sollten Skripte folgende Faktoren erkennen und anpassen:

| Kontext | Beispiel |
|---------|----------|
| OS | Windows, Linux, macOS |
| Interpreter | bash, pwsh, python, node, cmd.exe |
| Privilege | root, NT AUTHORITY\SYSTEM, Administrator, user |
| Architektur | x64, x86, arm64 |
| Tool-Versionen | `python --version`, `pwsh -v`, `node -v` |

**Nicht unterstützte Umgebungen:**
- Klare Nachricht ausgeben
- Mit `exit 1` beenden

## Beispiel: Service Restart (Linux)

```bash
#!/bin/bash
# *************************************************************************************
# Component: Service Restart [LIN] - DevWing Version
# Author: DevWing User
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

### Phase 1: Test-Version
Zuerst nur funktionale Test-Version:
- Kein Header, keine Formatierung
- Nur Core-Logik
- Schnell testbar

### Phase 2: Produktions-Version
Nach Bestätigung ("Funktioniert!"):
1. Fragen: "Soll ich die DevWing-Regeln anwenden?"
2. Nach Bestätigung: Full Version mit Header, Exit-Codes, Documentation
