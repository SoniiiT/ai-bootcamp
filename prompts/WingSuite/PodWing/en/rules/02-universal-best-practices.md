# Rule 02: Universal Best Practices (Security & Stability)

These rules apply to **all** container platforms (Docker, K8s, Podman).

## 1. Non-Root User
- **Rule**: Containers must **never** run as `root` unless technically unavoidable (e.g., system tools).
- **Action**: Always add `USER <uid>` or `USER nonroot` in Dockerfiles.
- **Kubernetes**: Set `runAsNonRoot: true` in SecurityContext.

## 2. Read-Only Filesystem
- **Rule**: The container's root filesystem should be read-only.
- **Exception**: Temporary data belongs in `/tmp` (mounted as `emptyDir` or Volume).
- **Benefit**: Prevents attackers from persisting malware.

## 3. Resource Limits
- **Rule**: No container without limits (CPU & Memory).
- **Danger**: A container without limits can starve the entire node (Noisy Neighbor).
- **Recommendation**: Set Requests (guaranteed) and Limits (cap).

## 4. Health Checks / Probes
- **Rule**: Every service needs a health check.
- **Docker**: `HEALTHCHECK CMD curl -f ...`
- **Kubernetes**: LivenessProbe (restart on deadlock) and ReadinessProbe (traffic only when ready).

## 5. No Secrets in Environment Variables
- **Rule**: Sensitive data (passwords, API keys) do not belong in plain text ENV vars (visible via `docker inspect`).
- **Solution**: Use Secret Management (K8s Secrets, Docker Swarm Secrets, Vault).
