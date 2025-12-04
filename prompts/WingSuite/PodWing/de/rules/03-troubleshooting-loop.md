# Regel 03: The Troubleshooting Loop

Wenn ein User ein Problem meldet, folge strikt diesem Diagnose-Pfad, um die Ursache einzugrenzen.

## Phase 1: Status & Logs (Was ist passiert?)
1. **Status prüfen**: Läuft der Container? Welchen Exit-Code hat er?
   - *K8s*: `kubectl get pod` / `kubectl describe pod`
   - *Docker*: `docker ps -a`
2. **Logs lesen**: Was hat die App zuletzt ausgegeben?
   - *K8s*: `kubectl logs <pod> --previous` (wenn abgestürzt)
   - *Docker*: `docker logs --tail 100 <id>`

## Phase 2: Ressourcen & Events (Warum ist es passiert?)
1. **Events**: Gab es Scheduling-Probleme oder OOMKills?
   - *K8s*: `kubectl get events --sort-by=.metadata.creationTimestamp`
2. **Ressourcen**: Ist der Node voll?
   - *K8s*: `kubectl top pod` / `kubectl top node`
   - *Docker*: `docker stats`

## Phase 3: Inspektion & Netzwerk (Tiefenanalyse)
1. **Shell-Zugriff**: Wenn der Container läuft, aber spinnt.
   - `kubectl exec -it <pod> -- /bin/sh`
2. **Netzwerk**: Ist der Port offen? DNS-Auflösung?
   - Teste von *innen* (im Container) und von *außen* (anderer Pod/Host).
   - Nutze Ephemeral Debug Container, wenn keine Shell vorhanden ist (`kubectl debug`).

## Phase 4: Konfiguration (Layer 8)
1. **Diff**: Was hat sich geändert? (Image-Tag, ConfigMap, Environment).
2. **Reproduktion**: Tritt der Fehler auch lokal auf?
