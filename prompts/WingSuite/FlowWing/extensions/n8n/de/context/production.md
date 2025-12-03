---
description: Umfassender Leitfaden für die Bereitstellung, Skalierung und Wartung von n8n in Produktionsumgebungen.
globs: **/*.json
---

# Produktions- & Skalierungs-Handbuch

Der Betrieb von n8n in der Produktion erfordert eine sorgfältige Konfiguration, um Zuverlässigkeit, Sicherheit und Leistung zu gewährleisten.

## 1. Bereitstellungsarchitektur

### Einzelinstanz (Standard)
*   **Komponenten**: n8n Container + Datenbank (Postgres).
*   **Vorteile**: Einfach, leicht zu verwalten.
*   **Nachteile**: Single Point of Failure. Wenn ein Workflow die CPU auslastet, reagiert die UI nicht mehr.
*   **Limit**: Nur vertikale Skalierung (mehr RAM/CPU hinzufügen).

### Queue Mode (Skalierung)
Für Umgebungen mit hohem Volumen verwenden Sie den **Queue Mode**.
*   **Komponenten**:
    1.  **Main Instance**: Handhabt Webhooks, UI und Zeitplanung. Leichtgewichtig.
    2.  **Worker Instances**: Führen die Workflows aus. Sie können 10+ Worker haben.
    3.  **Redis**: Message Broker, um Jobs von der Main Instance an die Worker zu übergeben.
    4.  **Datenbank**: Gemeinsamer Speicher für Workflow-Definitionen und Ausführungshistorie.
*   **Vorteile**:
    *   **Resilienz**: Wenn ein Worker abstürzt, bleibt die Main Instance online.
    *   **Skalierbarkeit**: Fügen Sie mehr Worker hinzu, um mehr Last zu bewältigen.
*   **Konfiguration**:
    *   `EXECUTIONS_MODE=queue`
    *   `QUEUE_BULL_REDIS_HOST=...`

---

## 2. Datenbank-Management

Die Datenbank ist der Flaschenhals von n8n.

### Empfohlene Datenbank
*   **PostgreSQL**: Die einzige empfohlene DB für die Produktion.
*   **SQLite**: **NIEMALS** in der Produktion verwenden. Sie sperrt die Datei während Schreibvorgängen, was unter Last zu "Database is locked"-Fehlern führt.

### Pruning (Kritisch)
Ausführungsdaten wachsen exponentiell. Sie **müssen** Pruning konfigurieren.
*   `EXECUTIONS_DATA_PRUNE=true`
*   `EXECUTIONS_DATA_MAX_AGE=168` (Stunden - z. B. 7 Tage)
*   `EXECUTIONS_DATA_PRUNE_MAX_COUNT=50000` (Maximal zu behaltende Ausführungen)

### Optimierung
*   **Disable Saving Progress**: Deaktivieren Sie in den Workflow-Einstellungen "Save Execution Progress". Dies verhindert das Schreiben in die DB nach jedem Node.
*   **Save Only Errors**: Setzen Sie `EXECUTIONS_DATA_SAVE_ON_SUCCESS=false` global. Speichern Sie nur fehlgeschlagene Ausführungen für das Debugging.

---

## 3. Binärdatenspeicher

Das Speichern von Dateien (PDFs, Bilder) in der Postgres-Datenbank bläht diese auf und verlangsamt Backups.

### S3 Storage
Konfigurieren Sie n8n so, dass Binärdaten in einem S3-kompatiblen Bucket (AWS S3, MinIO, DigitalOcean Spaces) gespeichert werden.
*   `N8N_DEFAULT_BINARY_DATA_MODE=s3`
*   `N8N_S3_BUCKET_NAME=my-n8n-files`
*   `N8N_S3_REGION=us-east-1`
*   `N8N_S3_ACCESS_KEY=...`
*   `N8N_S3_ACCESS_SECRET=...`

---

## 4. Umgebungsvariablen

Verwalten Sie die Konfiguration über `.env`-Dateien.

### Essenzielle Variablen
*   `WEBHOOK_URL`: Die öffentliche HTTPS-URL (z. B. `https://n8n.example.com`). Erforderlich für OAuth2 und Webhooks.
*   `N8N_ENCRYPTION_KEY`: **KRITISCH**. Generiert die Verschlüsselung für Credentials. Wenn dieser verloren geht, sind alle Credentials verloren.
*   `GENERIC_TIMEZONE`: z. B. `Europe/Berlin`.
*   `N8N_HOST`: Der Domainname.

### Sicherheits-Variablen
*   `N8N_BASIC_AUTH_ACTIVE=true`: Wenn keine Benutzerverwaltung verwendet wird.
*   `N8N_AUTH_EXCLUDE_ENDPOINTS`: Endpunkte, die offengelegt werden sollen (z. B. `/healthz`).
*   `N8N_SECURE_COOKIE=true`: Erzwingt HTTPS-Cookies.

---

## 5. Monitoring & Logging

### Prometheus Metriken
n8n stellt Metriken unter `/metrics` bereit (wenn aktiviert).
*   `N8N_METRICS_ENABLED=true`
*   **Schlüsselmetriken**:
    *   `n8n_workflow_execution_count_total`
    *   `n8n_workflow_execution_duration_seconds`
    *   `n8n_worker_active_count` (Queue Mode)

### Logging
*   `N8N_LOG_LEVEL=info` (Standard). Verwenden Sie `debug` nur zur Fehlersuche.
*   `N8N_LOG_OUTPUT=json`: Am besten für die Aufnahme in Splunk/Datadog.

---

## 6. Hochverfügbarkeit (HA)

### Webhook Processors
Im Queue Mode ist die "Main Instance" immer noch ein Single Point of Failure für den Empfang von Webhooks.
*   **Lösung**: Führen Sie mehrere "Webhook Processor"-Instanzen hinter einem Load Balancer aus.
*   **Konfig**: Führen Sie den n8n-Container mit dem Befehl `webhook` aus.
*   **Fluss**: Load Balancer -> Webhook Processors (x2) -> Redis -> Workers (x5).

---

## 7. Updates & Wartung

*   **Docker Tags**: Pinnen Sie Ihre Version (z. B. `n8n/n8n:1.15.0`). Verwenden Sie nicht `latest`.
*   **Backup**:
    1.  Dump der Postgres-Datenbank.
    2.  Backup des `.n8n`-Ordners (enthält den Encryption Key).
*   **Rollback**: Wenn ein Update etwas kaputt macht, kehren Sie zum Docker-Tag zurück und stellen Sie das DB-Backup wieder her (falls Schemaänderungen aufgetreten sind).
