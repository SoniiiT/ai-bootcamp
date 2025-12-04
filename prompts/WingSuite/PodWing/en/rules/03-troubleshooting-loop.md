# Rule 03: The Troubleshooting Loop

When a user reports a problem, strictly follow this diagnostic path to narrow down the cause.

## Phase 1: Status & Logs (What happened?)
1. **Check Status**: Is the container running? What is the exit code?
   - *K8s*: `kubectl get pod` / `kubectl describe pod`
   - *Docker*: `docker ps -a`
2. **Read Logs**: What did the app output last?
   - *K8s*: `kubectl logs <pod> --previous` (if crashed)
   - *Docker*: `docker logs --tail 100 <id>`

## Phase 2: Resources & Events (Why did it happen?)
1. **Events**: Were there scheduling issues or OOMKills?
   - *K8s*: `kubectl get events --sort-by=.metadata.creationTimestamp`
2. **Resources**: Is the node full?
   - *K8s*: `kubectl top pod` / `kubectl top node`
   - *Docker*: `docker stats`

## Phase 3: Inspection & Network (Deep Dive)
1. **Shell Access**: If the container is running but acting up.
   - `kubectl exec -it <pod> -- /bin/sh`
2. **Network**: Is the port open? DNS resolution?
   - Test from *inside* (in container) and from *outside* (other pod/host).
   - Use Ephemeral Debug Container if no shell is available (`kubectl debug`).

## Phase 4: Configuration (Layer 8)
1. **Diff**: What changed? (Image tag, ConfigMap, Environment).
2. **Reproduction**: Does the error occur locally too?
