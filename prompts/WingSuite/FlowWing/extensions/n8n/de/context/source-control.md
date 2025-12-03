---
description: Umfassender Leitfaden zur Verwaltung von n8n-Workflows mit Git-basierter Versionskontrolle.
globs: **/*.json
---

# Versionskontroll- & Umgebungs-Handbuch

Die Source Control-Funktion von n8n ermöglicht es Ihnen, Ihre Workflows zu versionieren, sicher zusammenzuarbeiten und einen ordnungsgemäßen SDLC (Software Development Life Cycle) unter Verwendung von Git zu implementieren.

## Kernkonzepte

### Das "Eine Instanz = Ein Branch" Modell
n8n unterstützt nicht das dynamische Wechseln von Branches innerhalb einer einzelnen Instanz wie eine IDE.
*   **Dev Instance**: Verbunden mit dem `feature` oder `development` Branch.
*   **Prod Instance**: Verbunden mit dem `main` oder `production` Branch.

### Was wird synchronisiert?
*   **Synchronisiert**: Workflows, Tags, Variablen.
*   **NICHT Synchronisiert**: Credentials, Ausführungshistorie, Benutzerkonten.
    *   *Grund*: Sicherheit. Sie wollen Ihre Produktions-Stripe-Schlüssel nicht in Ihrem Git-Repo haben.

---

## Einrichtungsanleitung

### 1. Git-Repository-Vorbereitung
Erstellen Sie ein dediziertes Repository (z. B. `n8n-workflows`). Vermischen Sie dies nicht mit anderem Anwendungscode.

### 2. SSH-Konfiguration (Empfohlen)
SSH ist stabiler als HTTPS für automatisierte Operationen.
1.  **Schlüssel generieren**: In n8n (**Settings > Source Control**), klicken Sie auf "Generate SSH Key".
2.  **Zu Git hinzufügen**: Gehen Sie zu Ihrem Git-Anbieter (GitHub/GitLab) -> Repository Settings -> **Deploy Keys**.
3.  **Berechtigungen**: Stellen Sie sicher, dass der Schlüssel **Write Access** hat.

### 3. Instanz-Konfiguration

#### Development Instance
*   **Branch**: `development` (oder `main`, wenn Sie ein Solo-Entwickler sind).
*   **Read-Only**: `Off`.
*   **Workflow**: Sie bauen hier, committen und pushen.

#### Production Instance
*   **Branch**: `production`.
*   **Read-Only**: `On` (Geschützt).
*   **Workflow**: Sie **Pullen** hier nur Änderungen. Keine direkten Bearbeitungen erlaubt.

---

## Der Deployment-Workflow

### Schritt 1: Entwicklung
1.  Erstellen Sie einen Workflow in der **Dev Instance**.
2.  Testen Sie ihn gründlich.
3.  **Commit**: Gehen Sie zu **Source Control**. Sie sehen die Liste der "Changes".
4.  **Push**: Geben Sie eine Nachricht ein ("Feat: Added User Sync") und klicken Sie auf **Commit & Push**.

### Schritt 2: Merge (Extern)
1.  Gehen Sie zu Ihrem Git-Anbieter (GitHub/GitLab).
2.  Öffnen Sie einen **Pull Request** (PR) von `development` nach `production`.
3.  Überprüfen Sie das JSON-Diff (schwer zu lesen, aber achten Sie auf massive Löschungen).
4.  **Mergen** Sie den PR.

### Schritt 3: Deployment
1.  Gehen Sie zur **Prod Instance**.
2.  Gehen Sie zu **Source Control**.
3.  Sie sehen einen "Updates Available"-Indikator.
4.  Klicken Sie auf **Pull**.
5.  Die neuen Workflows sind jetzt live in der Produktion.

---

## Umgang mit Credentials

Da Credentials nicht synchronisiert werden, müssen Sie diese manuell verwalten oder External Secrets verwenden.

### Die "Gleicher Name" Strategie
1.  **Dev**: Erstellen Sie ein Credential namens `Stripe API` mit Testschlüsseln.
2.  **Prod**: Erstellen Sie ein Credential namens `Stripe API` mit Live-Schlüsseln.
3.  **Workflow**: Der Workflow referenziert das Credential über die **ID**.
    *   *Problem*: IDs sind zufällige UUIDs. Wenn Sie sie separat erstellen, stimmen die IDs nicht überein.
    *   *Lösung*: n8n versucht, nach **Name** zu mappen, wenn die ID fehlt, aber das ist fragil.
    *   *Best Practice*: Verwenden Sie **External Secrets** (Vault), sodass der Workflow nur `{{ $secrets.stripe.key }}` referenziert. Dies abstrahiert die Credential-ID vollständig.

---

## Konfliktlösung

**Szenario**: Zwei Entwickler bearbeiten denselben Workflow auf der Dev-Instanz.
*   **Ergebnis**: Das letzte Speichern gewinnt in der UI.
*   **Git-Konflikt**: Wenn Entwickler A pusht und Entwickler B später versucht zu pushen, ohne zu pullen, blockiert n8n den Push.
*   **Lösung**:
    1.  **Force Push**: Überschreibt das Remote (Gefährlich).
    2.  **Pull & Resolve**: n8n versucht, JSON automatisch zusammenzuführen. Wenn es fehlschlägt, könnten Sie Änderungen verlieren.
    *   *Rat*: Koordinieren Sie sich mit Ihrem Team. "Ich arbeite am User Sync Workflow, fass ihn nicht an."

---

## Rollback-Strategie

Wenn Sie einen Fehler in die Produktion deployen:
1.  **Git Revert**: In GitHub/GitLab, reverten Sie den Merge-Commit auf dem `production` Branch.
2.  **Pull**: In der n8n Prod-Instanz, klicken Sie auf **Pull**.
3.  Die Workflows kehren zum vorherigen Zustand zurück.

---

## Best Practices

1.  **Commit Often**: Warten Sie nicht bis zum Ende der Woche. Kleine Commits sind sicherer.
2.  **Aussagekräftige Nachrichten**: "Update workflow" ist nutzlos. Verwenden Sie "Fix: Handle 404 error in HubSpot node".
3.  **Tags**: Verwenden Sie Tags, um Workflows zu gruppieren. Tags werden synchronisiert, sodass Ihre Organisationsstruktur erhalten bleibt.
4.  **Variablen**: Verwenden Sie `$vars` für umgebungsspezifische Konfigurationen (z. B. `API_BASE_URL`). Definieren Sie die Variable in beiden Instanzen, aber mit unterschiedlichen Werten.
