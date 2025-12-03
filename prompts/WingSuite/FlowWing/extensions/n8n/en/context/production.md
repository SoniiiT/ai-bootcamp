---
description: Comprehensive guide for deploying, scaling, and maintaining n8n in production environments.
globs: **/*.json
---

# Production & Scaling Guide

Running n8n in production requires careful configuration to ensure reliability, security, and performance.

## 1. Deployment Architecture

### Single Instance (Standard)
*   **Components**: n8n Container + Database (Postgres).
*   **Pros**: Simple, easy to manage.
*   **Cons**: Single point of failure. If a workflow spikes CPU, the UI becomes unresponsive.
*   **Limit**: Vertical scaling only (add more RAM/CPU).

### Queue Mode (Scaling)
For high-volume environments, use **Queue Mode**.
*   **Components**:
    1.  **Main Instance**: Handles Webhooks, UI, and Scheduling. Lightweight.
    2.  **Worker Instances**: Execute the workflows. You can have 10+ workers.
    3.  **Redis**: Message broker to pass jobs from Main to Workers.
    4.  **Database**: Shared storage for workflow definitions and execution history.
*   **Pros**:
    *   **Resilience**: If a worker crashes, the main instance stays up.
    *   **Scalability**: Add more workers to handle more load.
*   **Configuration**:
    *   `EXECUTIONS_MODE=queue`
    *   `QUEUE_BULL_REDIS_HOST=...`

---

## 2. Database Management

The database is the bottleneck of n8n.

### Recommended Database
*   **PostgreSQL**: The only recommended DB for production.
*   **SQLite**: **NEVER** use in production. It locks the file during writes, causing "Database is locked" errors under load.

### Pruning (Critical)
Execution data grows exponentially. You **must** configure pruning.
*   `EXECUTIONS_DATA_PRUNE=true`
*   `EXECUTIONS_DATA_MAX_AGE=168` (Hours - e.g., 7 days)
*   `EXECUTIONS_DATA_PRUNE_MAX_COUNT=50000` (Max executions to keep)

### Optimization
*   **Disable Saving Progress**: In Workflow Settings, uncheck "Save Execution Progress". This prevents writing to the DB after every node.
*   **Save Only Errors**: Set `EXECUTIONS_DATA_SAVE_ON_SUCCESS=false` globally. Only save failed executions for debugging.

---

## 3. Binary Data Storage

Storing files (PDFs, Images) in the Postgres database bloats it and slows down backups.

### S3 Storage
Configure n8n to store binary data in an S3-compatible bucket (AWS S3, MinIO, DigitalOcean Spaces).
*   `N8N_DEFAULT_BINARY_DATA_MODE=s3`
*   `N8N_S3_BUCKET_NAME=my-n8n-files`
*   `N8N_S3_REGION=us-east-1`
*   `N8N_S3_ACCESS_KEY=...`
*   `N8N_S3_ACCESS_SECRET=...`

---

## 4. Environment Variables

Manage configuration via `.env` files.

### Essential Vars
*   `WEBHOOK_URL`: The public HTTPS URL (e.g., `https://n8n.example.com`). Required for OAuth2 and Webhooks.
*   `N8N_ENCRYPTION_KEY`: **CRITICAL**. Generates the encryption for credentials. If lost, all credentials are lost.
*   `GENERIC_TIMEZONE`: e.g., `America/New_York`.
*   `N8N_HOST`: The domain name.

### Security Vars
*   `N8N_BASIC_AUTH_ACTIVE=true`: If not using user management.
*   `N8N_AUTH_EXCLUDE_ENDPOINTS`: Endpoints to expose (e.g., `/healthz`).
*   `N8N_SECURE_COOKIE=true`: Force HTTPS cookies.

---

## 5. Monitoring & Logging

### Prometheus Metrics
n8n exposes metrics at `/metrics` (if enabled).
*   `N8N_METRICS_ENABLED=true`
*   **Key Metrics**:
    *   `n8n_workflow_execution_count_total`
    *   `n8n_workflow_execution_duration_seconds`
    *   `n8n_worker_active_count` (Queue mode)

### Logging
*   `N8N_LOG_LEVEL=info` (Default). Use `debug` only for troubleshooting.
*   `N8N_LOG_OUTPUT=json`: Best for ingestion into Splunk/Datadog.

---

## 6. High Availability (HA)

### Webhook Processors
In Queue Mode, the "Main Instance" is still a single point of failure for receiving webhooks.
*   **Solution**: Run multiple "Webhook Processor" instances behind a Load Balancer.
*   **Config**: Run the n8n container with the command `webhook`.
*   **Flow**: Load Balancer -> Webhook Processors (x2) -> Redis -> Workers (x5).

---

## 7. Updates & Maintenance

*   **Docker Tags**: Pin your version (e.g., `n8n/n8n:1.15.0`). Do not use `latest`.
*   **Backup**:
    1.  Dump the Postgres Database.
    2.  Backup the `.n8n` folder (contains the Encryption Key).
*   **Rollback**: If an update breaks something, revert the Docker tag and restore the DB backup (if schema changes occurred).
