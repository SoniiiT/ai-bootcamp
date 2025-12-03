# Workflow Automation Tech Stack

## Übersicht
Dieser Kontext definiert die allgemeinen Technologien und Konzepte, mit denen FlowWing arbeitet. Er dient als Basis für Automatisierungsaufgaben, bevor spezifische Plattform-Erweiterungen angewendet werden.

## Kernkonzepte

### Trigger (Auslöser)
- **Webhook**: Echtzeit-Datenübertragung via HTTP POST.
- **Polling**: Regelmäßiges Abfragen auf Änderungen.
- **Zeitplan (Schedule)**: Zeitbasierte Ausführung (Cron).
- **App-Event**: Spezifisches Ereignis in einer verbundenen App (z.B. "Neue E-Mail").

### Aktionen
- **CRUD-Operationen**: Erstellen, Lesen, Aktualisieren, Löschen von Daten.
- **Transformation**: Änderung der Datenstruktur (JSON parsen, String-Manipulation).
- **Logik**: Wenn/Dann, Switch, Warten, Beenden.

### Datenformate
- **JSON**: Der Standard für die meisten modernen APIs und Workflow-Engines (n8n, Logic Apps).
- **YAML**: Oft genutzt für Definition-as-Code (Kestra, GitHub Actions).
- **Expressions**: Plattformspezifische Formeln (Power Automate Expressions, Excel-ähnliche Formeln).

## Unterstützte Plattformen (Allgemeinwissen)

### Low-Code / No-Code
- **Microsoft Power Automate**: Fokus auf Unternehmen, Microsoft 365 Integration.
- **Zapier**: Fokus auf Konsumenten/KMU, riesige Konnektor-Bibliothek.
- **Make (ehemals Integromat)**: Visuell, komplexe Datenszenarien.

### Developer / Self-Hosted
- **n8n**: Node-basiert, visuell aber code-freundlich (JavaScript).
- **Kestra**: Orchestration-as-Code, Java/YAML-basiert.
- **Node-RED**: IoT und ereignisgesteuert, flussbasierte Programmierung.

## Best Practices

1. **Idempotenz**: Sicherstellen, dass Flows mehrfach laufen können, ohne Duplikate zu erzeugen.
2. **Fehlerbehandlung**: Immer "On Error"-Pfade oder Try/Catch-Scopes implementieren.
3. **Umgebungsvariablen**: Variablen für Konfigurationen (URLs, E-Mails) nutzen statt Hardcoding.
