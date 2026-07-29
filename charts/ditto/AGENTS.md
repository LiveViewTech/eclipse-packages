## Context

Deprecated local Helm chart for Eclipse Ditto digital twins. Upstream chart lives in eclipse-ditto/ditto and is published via Docker Hub OCI.

## Tech

- Helm chart `ditto` with `deprecated: true` in `Chart.yaml`
- Bitnami MongoDB subchart dependency (`mongodb.enabled`)
- Templates for gateway, things, policies, connectivity, nginx, swaggerui

## Architecture

Multi-service Ditto deployment behind nginx; MongoDB for persistence. This copy is not the chart `packages/cloud2edge` depends on (that uses `oci://registry-1.docker.io/eclipse`).

## Patterns

- DO prefer `oci://registry-1.docker.io/eclipse/ditto` for new installs (`README.md`)
- DON'T assume edits here ship with cloud2edge — check `packages/cloud2edge/Chart.yaml` dependency source
- DON'T remove without checking Jenkins still packages `charts/*/Chart.yaml` (`.jenkins/Jenkinsfile`)

## Key Files

- `Chart.yaml` — deprecation flag and MongoDB dep
- `README.md` — relocation notice and OCI install command
- `values.yaml` — legacy chart configuration
- `templates/gateway-deployment.yaml` — API gateway workload
