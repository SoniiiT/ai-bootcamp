# Regel 02: Universal Best Practices (Security & Stability)

Diese Regeln gelten für **alle** Container-Plattformen (Docker, K8s, Podman).

## 1. Non-Root User
- **Regel**: Container dürfen **niemals** als `root` laufen, es sei denn, es ist technisch unvermeidbar (z.B. System-Tools).
- **Aktion**: Füge in Dockerfiles immer `USER <uid>` oder `USER nonroot` hinzu.
- **Kubernetes**: Setze `runAsNonRoot: true` im SecurityContext.

## 2. Read-Only Filesystem
- **Regel**: Das Root-Dateisystem des Containers sollte schreibgeschützt sein.
- **Ausnahme**: Temporäre Daten gehören in `/tmp` (als `emptyDir` oder Volume gemountet).
- **Vorteil**: Verhindert, dass Angreifer Malware persistieren können.

## 3. Resource Limits
- **Regel**: Kein Container ohne Limits (CPU & Memory).
- **Gefahr**: Ein Container ohne Limits kann den gesamten Node lahmlegen (Noisy Neighbor).
- **Empfehlung**: Setze Requests (garantiert) und Limits (Obergrenze).

## 4. Health Checks / Probes
- **Regel**: Jeder Service braucht einen Health Check.
- **Docker**: `HEALTHCHECK CMD curl -f ...`
- **Kubernetes**: LivenessProbe (Neustart bei Deadlock) und ReadinessProbe (Traffic erst wenn bereit).

## 5. Keine Secrets in Environment-Variablen
- **Regel**: Sensible Daten (Passwörter, API-Keys) gehören nicht im Klartext in ENV-Vars (sichtbar via `docker inspect`).
- **Lösung**: Nutze Secret-Management (K8s Secrets, Docker Swarm Secrets, Vault).
