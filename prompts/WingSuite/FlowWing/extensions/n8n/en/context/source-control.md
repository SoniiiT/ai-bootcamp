---
description: Comprehensive guide to managing n8n workflows with Git-based Source Control.
globs: **/*.json
---

# Source Control & Environments Guide

n8n's Source Control feature allows you to version your workflows, collaborate safely, and implement a proper SDLC (Software Development Life Cycle) using Git.

## Core Concepts

### The "One Instance = One Branch" Model
n8n does not support switching branches dynamically within a single instance like an IDE.
*   **Dev Instance**: Connected to the `feature` or `development` branch.
*   **Prod Instance**: Connected to the `main` or `production` branch.

### What is Synced?
*   **Synced**: Workflows, Tags, Variables.
*   **NOT Synced**: Credentials, Execution History, User Accounts.
    *   *Reason*: Security. You don't want your production Stripe keys in your git repo.

---

## Setup Guide

### 1. Git Repository Preparation
Create a dedicated repository (e.g., `n8n-workflows`). Do not mix this with other application code.

### 2. SSH Configuration (Recommended)
SSH is more stable than HTTPS for automated operations.
1.  **Generate Key**: In n8n (**Settings > Source Control**), click "Generate SSH Key".
2.  **Add to Git**: Go to your Git provider (GitHub/GitLab) -> Repository Settings -> **Deploy Keys**.
3.  **Permissions**: Ensure the key has **Write Access**.

### 3. Instance Configuration

#### Development Instance
*   **Branch**: `development` (or `main` if you are a solo dev).
*   **Read-Only**: `Off`.
*   **Workflow**: You build here, commit, and push.

#### Production Instance
*   **Branch**: `production`.
*   **Read-Only**: `On` (Protected).
*   **Workflow**: You only **Pull** changes here. No direct edits allowed.

---

## The Deployment Workflow

### Step 1: Development
1.  Create a workflow in the **Dev Instance**.
2.  Test it thoroughly.
3.  **Commit**: Go to **Source Control**. You will see the "Changes" list.
4.  **Push**: Enter a message ("Feat: Added User Sync") and click **Commit & Push**.

### Step 2: Merge (External)
1.  Go to your Git Provider (GitHub/GitLab).
2.  Open a **Pull Request** (PR) from `development` to `production`.
3.  Review the JSON diff (hard to read, but check for massive deletions).
4.  **Merge** the PR.

### Step 3: Deployment
1.  Go to the **Prod Instance**.
2.  Go to **Source Control**.
3.  You will see a "Updates Available" indicator.
4.  Click **Pull**.
5.  The new workflows are now live in Production.

---

## Handling Credentials

Since credentials are not synced, you must manage them manually or use External Secrets.

### The "Same Name" Strategy
1.  **Dev**: Create a credential named `Stripe API` with test keys.
2.  **Prod**: Create a credential named `Stripe API` with live keys.
3.  **Workflow**: The workflow references the credential by **ID**.
    *   *Problem*: IDs are random UUIDs. If you create them separately, IDs won't match.
    *   *Solution*: n8n attempts to map by **Name** if the ID is missing, but it's fragile.
    *   *Best Practice*: Use **External Secrets** (Vault) so the workflow just references `{{ $secrets.stripe.key }}`. This abstracts the credential ID entirely.

---

## Conflict Resolution

**Scenario**: Two developers edit the same workflow on the Dev instance.
*   **Result**: The last save wins in the UI.
*   **Git Conflict**: If Developer A pushes, and Developer B tries to push later without pulling, n8n will block the push.
*   **Fix**:
    1.  **Force Push**: Overwrites the remote (Dangerous).
    2.  **Pull & Resolve**: n8n tries to auto-merge JSON. If it fails, you might lose changes.
    *   *Advice*: Coordinate with your team. "I'm working on the User Sync workflow, don't touch it."

---

## Rollback Strategy

If you deploy a bug to Production:
1.  **Git Revert**: In GitHub/GitLab, revert the merge commit on the `production` branch.
2.  **Pull**: In the n8n Prod instance, click **Pull**.
3.  The workflows revert to the previous state.

---

## Best Practices

1.  **Commit Often**: Don't wait until the end of the week. Small commits are safer.
2.  **Descriptive Messages**: "Update workflow" is useless. Use "Fix: Handle 404 error in HubSpot node".
3.  **Tags**: Use tags to group workflows. Tags are synced, so your organization structure persists.
4.  **Variables**: Use `$vars` for environment-specific config (e.g., `API_BASE_URL`). Define the variable in both instances, but with different values.
