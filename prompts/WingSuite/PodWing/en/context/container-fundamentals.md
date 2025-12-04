# Container Fundamentals & OCI Standards

## Open Container Initiative (OCI)
PodWing operates according to OCI standards. This means:
- **Images**: We prefer OCI-compliant images that run on any runtime (Docker, Podman, Kubernetes).
- **Runtimes**: We distinguish between High-Level Runtimes (Docker, Podman) and Low-Level Runtimes (runc, crun).

## Core Concepts

### 1. Immutability
Containers should be treated as immutable artifacts.
- **Anti-Pattern**: `apt-get update` or configuration changes inside a running container.
- **Best Practice**: Build a new image and redeploy.

### 2. Statelessness
Containers should be stateless (where possible).
- Data belongs in **Volumes** or external databases, not in the container layer.
- This allows for easy scaling and restarting.

### 3. Layer Caching & Optimization
- **Order**: Changes that rarely change (system libs) go at the top of the Dockerfile. Changes that change often (source code) go at the bottom.
- **Multi-Stage Builds**: Use build stages to keep compilers and build tools out of the final image (reducing attack surface and size).

### 4. The Sidecar Pattern
A helper container running in the same Pod/Namespace as the main container.
- **Use-Case**: Log shipping, Proxy (Service Mesh), Config reloading.
- **Rule**: Sidecars should be tightly coupled to the lifecycle of the main application.

### 5. Init Containers
Containers that start before the main application and must complete successfully.
- **Use-Case**: Waiting for database availability, preparing filesystem permissions.
