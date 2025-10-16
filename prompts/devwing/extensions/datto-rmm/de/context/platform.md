# Datto RMM - Plattform-Kontext

## Überblick

**Datto RMM** (Remote Monitoring and Management) ist eine cloudbasierte RMM-Plattform von **Kaseya**.
Sie ermöglicht IT-Dienstleistern die zentrale Verwaltung, Überwachung und Automatisierung von IT-Infrastrukturen.

## Unterstützte Plattformen

Datto RMM unterstützt folgende Betriebssysteme und Scripting-Umgebungen:

| Sprache | Ziel-Agents | Hinweise |
|---------|-------------|----------|
| PowerShell | Windows | Empfohlen für alle modernen Windows-Geräte |
| Batch (CMD) | Windows (Legacy) | Für ältere Windows-Agents ohne PowerShell |
| VBScript | Windows | Für spezielle Windows-Automatisierung und Legacy-Systeme |
| Shell (bash/sh) | Linux und macOS | Für Unix-basierte Agents |

### PowerShell-Versionen
- **Version 2.0**: Standard auf Windows 7, verfügbar für Windows XP/Server 2003
- **Version 5.0**: Standard auf Windows 10+
- **Kompatibilität**: Nicht alle Versionen haben gleiche Features
- **Best Practice**: PowerShell 2.0-Kompatibilität für breite OS-Unterstützung sicherstellen

**Mindestversion festlegen:**
```powershell
#Requires -Version 3
```
Diese Zeile muss die **erste Zeile** des Skripts sein. Ältere Versionen lehnen das Skript ohne Ausführung ab.

### PowerShell-Signierung (Self-Signed)
Für selbstsignierte PowerShell-Skripte:

**Voraussetzung:** Das Zertifikat muss auf dem Remote-Gerät im **Trusted Publishers Store** installiert sein.

**Prozess:**
1. Self-signed Zertifikat erstellen
2. Zertifikat mit Tool wie CertMgr im Trusted Publishers Certificate Store installieren
3. Skript in lokale .ps1-Datei kopieren
4. Mit Tool wie SignTool und dem Zertifikat signieren
5. Signierten Inhalt zurück in die Komponente kopieren

### Shell/Bash (Unix/macOS)
**Shebang-Zeile erforderlich:**
```bash
#!/bin/bash
```

**Wichtig:**
- Diese Zeile **muss** am Anfang jedes Unix-Skripts stehen
- Wählt den Command Interpreter (Bash ist am kompatibel über *nix-Systeme)
- macOS verwendet neuere Versionen ZSH, aber Bash bleibt für Skripte kompatibel
- **Nicht modifizieren**, es sei denn, du weißt genau, was du tust!

### Kommentare in Skripten
- **PowerShell/Shell**: `# Kommentar`
- **Batch**: `REM Kommentar` oder `:: Kommentar`
- **VBScript**: `' Kommentar`

### Batch-Skripte Best Practices
**Empfehlung:** Beginne Batch-Skripte mit `@echo off` für bessere Lesbarkeit der Ausgabe.

**Beispiel:**
```batch
@echo off
set time=Morning
echo Good %time%
```

**Ausgabe:**
```
Good Morning
```

Ohne `@echo off` würde jeder Befehl selbst auch ausgegeben, was die Ausgabe unübersichtlich macht.

## Komponenten-Kategorien

Datto RMM unterscheidet drei Hauptkategorien:

### 1. Applications
- **Zweck**: Installation oder Ausführung von Programmen
- **Features**: Kann Dateien enthalten, Component Credentials nutzen
- **Verwendung**: Software-Deployments, Installationen

### 2. Monitors
- **Zweck**: Überwachung von Systemzuständen
- **Verhalten**: Regelmäßige Ausführung zur Zustandsprüfung
- **Alerts**: Können Alarme bei bestimmten Bedingungen auslösen
- **⚠️ Wichtig**: Kategorie kann nach Erstellung nicht mehr geändert werden!

### 3. Scripts
- **Zweck**: Ausführung von Aktionen oder Automatisierungen
- **Verhalten**: On-Demand oder geplante Ausführung
- **Features**: Kann Dateien enthalten, Component Credentials nutzen
- **Verwendung**: Wartung, Updates, Konfiguration

## Ausführungskontext

### Berechtigungen
- **Windows**: Läuft als `NT AUTHORITY\SYSTEM` (LocalSystem Account)
- **Linux/macOS**: Läuft als `root`
- Voller Systemzugriff – Vorsicht bei Operationen!

### LocalSystem Account (Windows)
Der LocalSystem Account hat besondere Eigenschaften:

**Vorteile:**
- Umfassende Privilegien auf lokalem Endgerät
- Ausführung möglich, auch wenn niemand angemeldet ist
- Funktioniert auch mit Limited User-Rechten

**Einschränkungen:**
- **Kein sichtbarer Desktop**: Windows-Fenster sind verborgen und nicht interaktiv
  - Installer mit "Next"-Buttons hängen und warten auf nicht mögliche Eingabe
- **Keine virtuelle Tastatur/Maus**: Keine Makro-basierte Automation möglich
- **Kein Passwort**: Keine Netzwerk-Authentifizierung möglich
- **Keine Benutzer-Ressourcen**: Kein Zugriff auf gemappte Netzlaufwerke pro User

**Alternative:**
Quick Jobs laufen immer als LocalSystem. Um Skripte im Kontext des angemeldeten Users auszuführen:
- Scheduled Job verwenden
- Erweiterte Optionen unter "Execution" nutzen

### Laufzeitumgebung
- Skripte laufen auf Remote-Agents
- Keine Benutzerinteraktion möglich
- Stdout/Stderr werden zentral erfasst
- Timeout-Limits beachten (konfigurierbar pro Komponente)
- **LocalSystem-Test**: Empfohlen, Skripte als LocalSystem zu testen, um Datto RMM-Verhalten zu simulieren

### Ausführungsverzeichnis
Skripte werden in einem temporären **Package Directory** ausgeführt, das vom Agent erstellt wird:
- Enthält das Skript selbst
- Enthält alle angehängten Dateien (Attached Files)
- Enthält einfache Metadaten

**Dateireferenzierung:**
- Angehängte Dateien können **ohne Pfadangaben** referenziert werden
- Bei PowerShell: `datei.txt` (direkt)
- Bei Batch: `datei.txt` (direkt)
- Bei Shell: `datei.txt` (direkt)

## Component Level (Zugriffskontrolle)

Jede Komponente hat ein **Component Level**, das den Zugriff steuert:

### Level-Stufen
1. **Basic (1)** – Niedrigste Stufe
2. **Low (2)**
3. **Medium (3)**
4. **High (4)**
5. **Super (5)** – Höchste Stufe

### Zugriffskontrolle
- Benutzer sehen nur Komponenten auf oder unter ihrem Level
- Beispiel: User mit Level "Medium (3)" sieht nur Basic (1), Low (2) und Medium (3)
- Beim Bearbeiten können nur verfügbare Levels ausgewählt werden

## Attached Files

### Verwendung
- Nur für **Applications** und **Scripts** verfügbar
- Mehrere Dateien möglich
- **Max. Dateigröße**: 3 GB pro Datei
- **Max. Komponentengröße**: 5 GB total

### Wichtige Hinweise
- Dateiinhalte werden **nicht** von Datto RMM verifiziert
- Dateien können im Skript referenziert und genutzt werden
- Bei Upload-Fehlern wird Komponente ohne betroffene Dateien gespeichert

## Component Credentials

### Zweck
Nur für **Applications** und **Scripts** verfügbar.
Ermöglicht Verwendung von gecachten Credentials, die pro Site einzigartig sind.

### Verwendung
- Checkbox "Requires Component Credentials" aktivieren
- Credentials werden zur Laufzeit bereitgestellt
- Nützlich für Software-Installationen oder Operationen, die Authentifizierung benötigen

## Site-Beschränkung

### Optionen
- **All Sites**: Komponente kann auf alle Sites deployed werden
  - Erfordert Zugriff auf alle Sites im Account
- **Selected Sites**: Nur auf ausgewählte Sites beschränken
  - User ohne Zugriff auf diese Sites sehen die Komponente nicht
  
**⚠️ Wichtig**: Site-Beschränkung funktioniert nur für Quick Jobs und User Tasks!

## Agent Variables (Vordefinierte Variablen)

Datto RMM stellt automatisch mehrere Umgebungsvariablen bereit, die in jeder unterstützten Scripting-Sprache verfügbar sind:

### System-Variablen
- `CS_ACCOUNT_UID`: Eindeutige ID des Datto RMM Accounts für dieses Gerät
- `CS_CC_HOST`: Control Channel URI für Agent-Kommunikation mit Datto RMM
- `CS_CC_PORT1`: Port für Agent-Kommunikation (HTTPS 443)
- `CS_CSM_ADDRESS`: Web-Interface-Adresse für dieses Gerät
- `CS_DOMAIN`: Domain des lokalen Geräts (falls verbunden)
- `CS_PROFILE_DESC`: Beschreibung der Site, wo sich dieses Gerät befindet
- `CS_PROFILE_NAME`: Name der Site, wo sich dieses Gerät befindet
- `CS_PROFILE_PROXY_TYPE`: 0 oder 1 (Proxy-Server konfiguriert?)
- `CS_PROFILE_UID`: Eindeutige ID der Site für dieses Gerät
- `CS_WS_ADDRESS`: Web Service URI für Agent-Kommunikation mit Datto RMM

### UDF-Variablen (User-Defined Fields)
- `UDF_1` bis `UDF_30`: Benutzerdefinierte Felder des Geräts
- **Wichtig**: UDF-Daten sind nur zum Zeitpunkt des Push gültig
- Bei Änderungen im Portal: Erst nach Monitoring Policy Refresh verfügbar (mind. 1x täglich oder manuell)
- **Empfehlung**: UDF-Daten nur für selten ändernde Werte in Monitoren nutzen

### Verwendung
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

Datto RMM ermöglicht die Definition von **Input Variables**, die zur Laufzeit als Umgebungsvariablen injiziert werden.

### Verfügbare Typen
- **String/Value**: Freitext-Eingabe
- **Selection**: Dropdown-Liste mit vordefinierten Optionen
- **Boolean**: "true" oder "false" (als String!)
- **Date**: Datumsauswahl (String-Format)

### Verwendung in Skripten

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

### Sicherheitsaspekt
Input-Variablen werden in Skripten verwendet, aber ihre Werte sind bei lokaler Skript-Inspektion verborgen – nützlich für sensible Daten.

**⚠️ Wichtig:** Booleans werden als Strings übergeben ("true"/"false"), nicht als native Booleans!

## Output-Struktur

### Result-Block (Pflicht für Monitore)
```
<-Start Result->
STATUS=Message
<-End Result->
```

### Diagnostic-Block (Optional)
```
<-Start Diagnostic->
Additional diagnostic information
<-End Diagnostic->
```

**Regeln:**
- `STATUS=` ohne Leerzeichen nach `=`
- Marker müssen exakt übereinstimmen
- Diagnostics nur bei Alerts (exit 1)
- Max. 60 Sekunden für Diagnostic-Befehle

## Exit-Codes

- `exit 0` → **Erfolg** (kein Alert)
- `exit 1` → **Fehler** (Alert wird ausgelöst)

**Wichtig:** Jede Komponente muss mit einem dieser Codes enden!

## Post-Conditions

Post-Conditions sind Regeln, die nach Skript-Ausführung den Output evaluieren.

### Parameter
- **Warning Text**: Text, der zur Warnung führt (case-sensitive)
- **Qualifier**: 
  - `Is found in` – Warnung, wenn Text gefunden
  - `Is not found in` – Warnung, wenn Text nicht gefunden
- **Resource**: `StdOut` oder `StdErr`

### Verhalten
Wenn eine Post-Condition erfüllt ist, endet der Job mit **Warning**-Status, auch bei `exit 0`.

### Beispiel
Wenn `ERROR:` (case-sensitive) als Warning Text definiert ist:
- Datto RMM durchsucht die Skript-Ausgabe
- Bei Fund: Job endet mit **Warning**-Status
- Anzeige: Orange "Warning"-Label in Job-Ergebnissen und Job-Status-Spalte

**Best Practice:** Nutze konsistente Error-Marker wie "ERROR:" oder "FAILED:" für automatische Warnungen.

## Scope of Support (Datto RMM Support)

### Was Datto Support abdeckt:
✅ Troubleshooting von Jobs mit Skripten/Komponenten
✅ Untersuchung, wenn Skript lokal funktioniert, aber nicht in Datto RMM
✅ Plattform-bezogene Probleme bei Skript-Ausführung

### Was Datto Support NICHT abdeckt:
❌ Erstellung von Custom Scripts/Komponenten
❌ Debugging von Custom-Script-Logik
❌ Script-Entwicklung oder Code-Review

**Alternative:** Bei Bedarf an Unterstützung bei Script-/Komponenten-Erstellung → Account Manager für Professional Services Engagement kontaktieren.

## Komponenten-Metadaten

### Erforderliche Felder
- **Name**: Komponentenname (Pflichtfeld zum Speichern)
- **Description**: Kurze Beschreibung der Komponente
- **Category**: Applications, Monitors oder Scripts
- **Script Type**: PowerShell, Batch, Shell oder VBScript
- **Level**: Component Level (Basic bis Super)

### Optionale Felder
- **Icon**: 48x48 Pixel (PNG/JPG/GIF)
  - PNG/JPG werden automatisch skaliert
  - GIF wird nicht skaliert
  - Standard-Icon wird verwendet, wenn keins hochgeladen
- **Timeout**: Maximale Ausführungszeit in Sekunden
- **Sites**: Alle oder ausgewählte Sites
- **Variables**: Input-Variablen für Laufzeit-Konfiguration
- **Files**: Angehängte Dateien (nur Applications/Scripts)
- **Post-Conditions**: Warnungs-Regeln basierend auf Output

## Best Practices

### Sicherheit
- Keine sensiblen Daten in Logs
- Secrets über sichere Mechanismen übergeben
- Input-Validierung immer durchführen

### Performance
- Timeout-Limits beachten
- Diagnostics unter 60 Sekunden
- Effiziente Befehle nutzen

### Wartbarkeit
- Klare, beschreibende Kommentare
- Strukturierte Ausgaben
- Einheitliche Namenskonventionen

### Fehlerbehandlung
- Prerequisites prüfen
- Saubere Fehlermeldungen
- Immer mit korrektem Exit-Code beenden

## Plattform-URL

**Datto RMM** ist ein Produkt von **Kaseya**.
Weitere Informationen: https://rmm.datto.com/help/de/Content/0HOME/Home.htm
