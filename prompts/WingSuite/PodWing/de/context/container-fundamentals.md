# Container Fundamentals & OCI Standards

## Open Container Initiative (OCI)
PodWing arbeitet nach OCI-Standards. Das bedeutet:
- **Images**: Wir bevorzugen OCI-konforme Images, die auf jeder Runtime (Docker, Podman, Kubernetes) laufen.
- **Runtimes**: Wir unterscheiden zwischen High-Level Runtimes (Docker, Podman) und Low-Level Runtimes (runc, crun).

## Core-Konzepte

### 1. Immutability (Unveränderlichkeit)
Container sollten als unveränderliche Artefakte betrachtet werden.
- **Anti-Pattern**: `apt-get update` oder Konfigurationsänderungen im laufenden Container.
- **Best Practice**: Neues Image bauen und neu deployen.

### 2. Statelessness
Container sollten (wenn möglich) zustandslos sein.
- Daten gehören in **Volumes** oder externe Datenbanken, nicht in den Container-Layer.
- Dies ermöglicht einfaches Skalieren und Neustarten.

### 3. Layer Caching & Optimierung
- **Reihenfolge**: Änderungen, die sich selten ändern (System-Libs), kommen im Dockerfile nach oben. Änderungen, die sich oft ändern (Source Code), nach unten.
- **Multi-Stage Builds**: Nutzen Sie Build-Stages, um Compiler und Build-Tools nicht im finalen Image zu haben (Reduzierung der Angriffsfläche und Größe).

### 4. Das Sidecar Pattern
Ein Hilfs-Container läuft im selben Pod/Namespace wie der Haupt-Container.
- **Use-Case**: Log-Shipping, Proxy (Service Mesh), Config-Reloading.
- **Regel**: Sidecars sollten eng an den Lebenszyklus der Hauptanwendung gekoppelt sein.

### 5. Init Containers
Container, die vor der Hauptanwendung starten und erfolgreich beendet sein müssen.
- **Use-Case**: Warten auf Datenbank-Verfügbarkeit, Vorbereiten von Dateisystem-Rechten.
