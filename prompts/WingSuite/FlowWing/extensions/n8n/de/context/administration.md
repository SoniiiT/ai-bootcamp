# Administrations- & Sicherheits-Handbuch

Dieser umfassende Leitfaden behandelt die Administration, Sicherheitshärtung, Benutzerverwaltung und Wartung von selbst gehosteten n8n-Instanzen. Er richtet sich an DevOps-Ingenieure und Systemadministratoren.

## Benutzerverwaltung

n8n bietet ein rollenbasiertes Zugriffskontrollsystem (RBAC), um Benutzerberechtigungen und Zusammenarbeit zu verwalten.

### Benutzerrollen & Berechtigungen

| Rolle | Geltungsbereich | Fähigkeiten | Einschränkungen |
| :--- | :--- | :--- | :--- |
| **Instance Owner** | Global | Voller Zugriff auf alle Workflows, Zugangsdaten (Credentials) und Instanzeinstellungen. Kann alle Benutzer verwalten. | Kann nicht gelöscht werden (nur übertragen). |
| **Instance Admin** | Global | Kann Benutzer und Instanzeinstellungen verwalten sowie alle Workflows einsehen. | Kann den Instance Owner nicht löschen oder herabstufen. |
| **Member** | Global | Kann Workflows und Zugangsdaten erstellen. Kann Workflows mit anderen teilen. | Kann nicht auf Instanzeinstellungen zugreifen oder andere Benutzer verwalten. |

### Benutzerbereitstellung (Provisioning)

#### 1. CLI-Verwaltung
Sie können Benutzer direkt über die CLI verwalten, wenn Sie Shell-Zugriff auf den n8n-Container haben.

```bash
# Alle Benutzer auflisten
n8n user-management:user:list

# Einen neuen Benutzer erstellen
n8n user-management:user:create --email="user@example.com" --firstName="John" --lastName="Doe" --role="global:member"

# Passwort eines Benutzers zurücksetzen
n8n user-management:user:reset-password --email="user@example.com" --password="newsecurepassword"

# Rolle eines Benutzers ändern
n8n user-management:user:update --email="user@example.com" --role="global:admin"
```

#### 2. E-Mail-Einladungen (SMTP)
Um Einladungen über die Benutzeroberfläche zu ermöglichen, müssen Sie SMTP-Umgebungsvariablen konfigurieren.

```bash
# .env Konfiguration
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.sendgrid.net
N8N_SMTP_PORT=587
N8N_SMTP_USER=apikey
N8N_SMTP_PASS=SG.xxxx
N8N_SMTP_SENDER=n8n@yourdomain.com
N8N_SMTP_SSL=false
```

#### 3. SSO (Enterprise)
Enterprise-Pläne unterstützen SAML und OIDC für Single Sign-On.
*   **SAML**: Unterstützt Okta, Google Workspace, JumpCloud, usw.
*   **OIDC**: Unterstützt Auth0, Azure AD, Keycloak.
*   **Just-in-Time (JIT) Provisioning**: Benutzer werden automatisch bei der ersten Anmeldung erstellt.

---

## Sicherheitshärtung

Die Sicherung Ihrer n8n-Instanz ist entscheidend, insbesondere wenn sie dem öffentlichen Internet ausgesetzt ist.

### Authentifizierung & Zugriffskontrolle

#### Basic Auth (Global)
Wenn Sie das Benutzerverwaltungssystem nicht verwenden (z. B. im Einzelbenutzermodus), schützen Sie die gesamte Instanz mit Basic Auth.
*   `N8N_BASIC_AUTH_ACTIVE=true`
*   `N8N_BASIC_AUTH_USER=admin`
*   `N8N_BASIC_AUTH_PASSWORD=sicherespasswort`

#### Endpunktschutz
Standardmäßig macht n8n Webhook-Endpunkte öffentlich zugänglich. Sie können den Zugriff auf die UI und API einschränken.

*   **`N8N_ENDPOINT_RESTRICTIONS_ENABLED=true`**: Aktiviert die Einschränkungslogik.
*   **`N8N_AUTH_EXCLUDE_ENDPOINTS`**: Liste der Endpunkte, die von der Authentifizierung ausgeschlossen werden sollen (z. B. `/healthz`).

### Externe Geheimnisse (Vaults)
Speichern Sie sensible API-Schlüssel oder Datenbankpasswörter niemals direkt in der n8n-Datenbank, wenn möglich. Verwenden Sie einen Anbieter für externe Geheimnisse (External Secrets).

#### Unterstützte Anbieter
*   AWS Secrets Manager
*   Azure Key Vault
*   Google Cloud Secret Manager
*   HashiCorp Vault
*   Infisical

#### Konfigurationsschritte
1.  **Anbieter verbinden**: Gehen Sie zu **Settings > External Secrets** und fügen Sie Ihre Anbieter-Zugangsdaten hinzu.
2.  **Geheimnisse referenzieren**: Wechseln Sie in einem beliebigen Node-Credential-Feld den Eingabemodus auf **Expression**.
3.  **Syntax**:
    ```javascript
    // Syntax: {{ $secrets.<provider>.<secret_name> }}
    
    // AWS Secrets Manager (JSON Objekt)
    {{ $secrets.awsSecretsManager.myApiKey }}
    
    // HashiCorp Vault (KV Engine)
    {{ $secrets.vault.database_password }}
    
    // Infisical
    {{ $secrets.infisical.stripe_live_key }}
    ```

#### Profi-Tipps
*   **Caching**: n8n speichert Geheimnisse zwischen, um API-Aufrufe an den Vault zu reduzieren. Konfigurieren Sie `N8N_EXTERNAL_SECRETS_CACHE_TTL` (Standard: 300 Sekunden), um Sicherheit und Leistung abzuwägen.
*   **Umgebungsvariablen**: Sie können auch `{{ $env.MY_VAR }}` verwenden, um auf Umgebungsvariablen zuzugreifen, die in den Container injiziert wurden. Dies ist eine einfachere Alternative zu einem vollständigen Vault für statische Geheimnisse.

### Netzwerksicherheit
*   **Reverse Proxy**: Betreiben Sie n8n immer hinter einem Reverse Proxy (Nginx, Traefik, Caddy), der die SSL/TLS-Terminierung übernimmt.
*   **Webhook Tunnel**: Verwenden Sie für die lokale Entwicklung das Flag `--tunnel`. Stellen Sie für die Produktion sicher, dass `WEBHOOK_URL` auf Ihre öffentliche HTTPS-Domain gesetzt ist.
*   **CSP Header**: n8n setzt strikte Content Security Policy Header. Wenn Sie n8n in ein iFrame einbetten, müssen Sie `N8N_EDITOR_BASE_URL` konfigurieren und möglicherweise CSP-Einstellungen über Umgebungsvariablen anpassen (mit Vorsicht verwenden).

---

## Logging & Auditing

### Anwendungsprotokolle (Application Logs)
Standardprotokolle, die vom n8n-Prozess (Node.js) generiert werden.
*   **Format**: JSON oder Text (`N8N_LOG_OUTPUT=json`).
*   **Level**: `N8N_LOG_LEVEL` (debug, info, warn, error).
*   **Ziel**: stdout/stderr (Standard Docker-Logging).

### Log Streaming (Events)
n8n kann Ausführungsereignisse an externe Überwachungstools streamen. Dies unterscheidet sich von den Anwendungsprotokollen.

#### Ziele
*   **Syslog**: Standardprotokoll für die Log-Aggregation.
*   **PostHog**: Produktanalyse.
*   **Sentry**: Fehlerverfolgung.
*   **Webhook**: Generischer HTTP POST an einen beliebigen Endpunkt (z. B. Splunk, Datadog, Slack).

#### Konfigurierbare Ereignisse
| Ereignisname | Beschreibung |
| :--- | :--- |
| `workflow.started` | Ausgelöst, wenn eine Ausführung beginnt. |
| `workflow.finished` | Ausgelöst, wenn eine Ausführung erfolgreich abgeschlossen wird. |
| `workflow.failed` | Ausgelöst, wenn eine Ausführung fehlschlägt. |
| `node.started` | Ausgelöst, wenn ein spezifischer Node die Verarbeitung beginnt. |
| `node.finished` | Ausgelöst, wenn ein Node fertig ist. |
| `node.failed` | Ausgelöst, wenn ein Node einen Fehler meldet. |
| `audit.user.login` | Benutzeranmeldeereignisse. |
| `audit.workflow.created` | Workflow-Erstellungsereignisse. |

#### Webhook Payload Beispiel
```json
{
  "eventName": "workflow.failed",
  "workflowId": "123",
  "executionId": "456",
  "error": {
    "message": "API Rate Limit Exceeded",
    "node": "HTTP Request"
  },
  "timestamp": "2023-10-27T10:00:00Z"
}
```

### Audit Logs (Enterprise)
Enterprise-Pläne enthalten eine detaillierte Audit-Log-Ansicht in der Benutzeroberfläche.
*   **Verfolgt**: Wer hat was und wann getan.
*   **Umfang**: Benutzeranmeldungen, Zugriff auf Zugangsdaten, Workflow-Änderungen, Wiederholungen von Ausführungen.
*   **Aufbewahrung**: Konfigurierbarer Aufbewahrungszeitraum.

---

## Wartung & Betrieb

### Datenbankverwaltung
n8n verwendet eine Datenbank (SQLite, Postgres, MySQL), um Workflows, Zugangsdaten und den Ausführungsverlauf zu speichern.

#### Bereinigung von Ausführungsdaten (Pruning)
Ausführungsdaten wachsen schnell an. Sie **müssen** das Pruning konfigurieren, um einen Speicherüberlauf zu verhindern.

```bash
# Pruning aktivieren
EXECUTIONS_DATA_PRUNE=true

# Max. Alter der zu behaltenden Ausführungen (in Stunden)
EXECUTIONS_DATA_MAX_AGE=168  # 7 Tage

# Max. Anzahl der zu behaltenden Ausführungen (pro Workflow)
EXECUTIONS_DATA_PRUNE_MAX_COUNT=10000
```

#### Datenbankmigration
*   **SQLite**: Standard. Gut für Tests/geringe Last. Nicht für die Produktion empfohlen.
*   **PostgreSQL**: Empfohlen für die Produktion.
    *   Setzen Sie `DB_TYPE=postgresdb`.
    *   Setzen Sie `DB_POSTGRESDB_HOST`, `DB_POSTGRESDB_PORT`, `DB_POSTGRESDB_DATABASE`, `DB_POSTGRESDB_USER`, `DB_POSTGRESDB_PASSWORD`.

### Updates & Upgrades
*   **Docker Tagging**: Vermeiden Sie die Verwendung von `latest`. Pinnen Sie auf eine bestimmte Version (z. B. `n8n/n8n:1.15.0`), um unerwartete Breaking Changes zu vermeiden.
*   **Backup**: Sichern Sie immer Ihre Datenbank und das `.n8n`-Verzeichnis (Verschlüsselungsschlüssel), bevor Sie ein Upgrade durchführen.
*   **Rollback**: Wenn ein Upgrade fehlschlägt, kehren Sie zum vorherigen Docker-Image-Tag zurück und stellen Sie das Datenbank-Backup wieder her.

### Öffentliche API
n8n stellt eine öffentliche API zur Verfügung, um die Instanz programmgesteuert zu verwalten.
*   **Dokumentation**: `http://<ihre-instanz>/api/v1/docs`
*   **Authentifizierung**: Erfordert einen API-Schlüssel (generieren unter **Settings > API**).
*   **Fähigkeiten**:
    *   Exportieren/Importieren von Workflows.
    *   Verwalten von Zugangsdaten.
    *   Auslösen von Workflows.
    *   Abrufen des Ausführungsstatus.

```javascript
// Beispiel: Workflow über öffentliche API auslösen
const response = await fetch('https://n8n.example.com/api/v1/executions', {
  method: 'POST',
  headers: {
    'X-N8N-API-KEY': 'ihr-api-schluessel'
  },
  body: JSON.stringify({
    workflowId: '123'
  })
});
```

## Fehlerbehebung bei häufigen Admin-Problemen

### "Workflow is locked"
*   **Ursache**: Ein anderer Benutzer bearbeitet den Workflow, oder eine Browsersitzung wurde nicht ordnungsgemäß geschlossen.
*   **Lösung**: Bitten Sie den Benutzer, den Tab zu schließen, oder warten Sie auf das Timeout der Sperre. Als Admin können Sie die Übernahme erzwingen (Enterprise).

### "Execution data is missing"
*   **Ursache**: Pruning-Einstellungen haben die Daten gelöscht, oder `EXECUTIONS_DATA_SAVE_ON_SUCCESS=false`.
*   **Lösung**: Überprüfen Sie `EXECUTIONS_DATA_SAVE_ON_SUCCESS` (Standard ist `true`). Bei Instanzen mit hohem Volumen setzen Sie dies auf `false` und speichern nur Fehler (`EXECUTIONS_DATA_SAVE_ON_ERROR=true`).

### "n8n keeps restarting"
*   **Ursache**: OOM (Out of Memory - Arbeitsspeicher voll).
*   **Lösung**:
    1.  Überprüfen Sie die Docker-Speicherlimits.
    2.  Reduzieren Sie die `EXECUTIONS_PROCESS=main` Parallelität.
    3.  Wechseln Sie zu `EXECUTIONS_MODE=queue` (Worker-Modus), um die Last zu verteilen.
