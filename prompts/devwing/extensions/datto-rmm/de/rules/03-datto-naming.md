# Regel 6 - Komponenten-Benennung

## Zweck
Diese Regel definiert, wie Datto RMM Komponenten benannt werden sollen.

## Grundprinzip

Komponentennamen sollten **prägnant, beschreibend und professionell** sein,
**ohne** die Wörter "Script" oder "Monitor" zu enthalten.

**Warum?**
Datto RMM erkennt den Komponententyp automatisch – diese Begriffe sind redundant.

## Benennungs-Richtlinien

### ✅ DO: Fokus auf Funktion

- **Was** die Komponente tut – nicht **wie** sie implementiert ist
- Redundante oder technische Suffixe vermeiden: "Script", "Monitor", "Check", "Task"
- Klare, aktionsorientierte Titel, die Zweck oder Zielsystem beschreiben

### ✅ DO: Title Case verwenden

- Jeden Hauptbegriff großschreiben
- Beispiel: `Windows 11 Upgrade`, nicht `windows 11 upgrade`

### ✅ DO: Kurz halten

- Ideal: **unter 5 Wörtern**
- Keine unnötigen Satzzeichen oder Versionsnummern im Titel

## Beispiele

### ✅ Korrekte Namen

| Komponentenname | Erklärung |
|-----------------|-----------|
| Windows 11 Upgrade | Führt Upgrade zu Windows 11 durch oder validiert es |
| Windows 11 Driver Fix | Repariert oder reinstalliert Treiber nach Upgrade |
| Hyper-V VM State | Prüft und meldet aktuellen VM-Zustand |
| Disk Space Alert | Überwacht Speicherplatz und löst Alert bei niedrigem Stand aus |
| Service Restart Automation | Stellt sicher, dass kritische Services laufen und startet sie bei Bedarf neu |

### ❌ Inkorrekte Namen

| Inkorrekt | Problem |
|-----------|---------|
| Windows 11 Upgrade Script | Redundant – "Script" fügt keinen Mehrwert hinzu |
| Hyper-V VM State Monitor | Redundant – Datto RMM weiß bereits, dass es ein Monitor ist |
| Disk Space Check Script | "Check" und "Script" wiederholen beide den Funktionstyp |
| Service Status Checker | "Checker" ist redundant |
| File Monitoring Script | "Monitoring" und "Script" sind überflüssig |

## Zusätzliche Hinweise

- **Title Case** verwenden (jeden Hauptbegriff großschreiben)
- **Keine Versionsnummern** im Titel (z.B. nicht "v1.0")
- **Keine unnötige Interpunktion** (z.B. keine Bindestriche am Ende)
- **Kurz und prägnant** – idealerweise unter 5 Wörter

## Anwendungsbereich

Diese Regel gilt ausschließlich für **Datto RMM Komponenten** (Monitor und Component Naming) von Kaseya.

Für andere Skript-Typen oder Plattformen können abweichende Konventionen gelten.
