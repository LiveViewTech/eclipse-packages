## Context

Helm chart for Eclipse hawkBit update server: Deployments, MariaDB/RabbitMQ deps, optional microservices/GUI, and a DB migrate Job.

## Tech

- Helm chart `hawkbit` (appVersion from `Chart.yaml`)
- Images: `hawkbit/hawkbit-update-server`, `hawkbit/hawkbit-repository-jpa-init`
- Optional deps: `mariadb`, `rabbitmq` (OCI cloudpirates; gated by values)
- Optional Vault Agent sidecar hooks via `vaultAgent` in `values.yaml`

## Architecture

Helm renders templates under `templates/` → Kubernetes Deployments/Services/Secrets. On install/upgrade, `templates/db-migrate-hook.yaml` runs as a Helm pre-install/pre-upgrade (and ArgoCD PreSync) Job with `HAWKBIT_DB_MODE=migrate` before app pods. Default `updateStrategy.type` is `Recreate`.

## Patterns

- DO set real credentials / `existingSecret` for auth and DB — defaults include `auth.password: "{noop}admin"` (`values.yaml`)
- DO keep migrate Job and Deployments aligned when changing DB URL/credential wiring (`templates/db-migrate-hook.yaml`, `templates/deployment.yaml`)
- DON'T assume CI golden tests work without `$HOME/src/argocd/values/infrastructure/hawkbit` (`test/render-test.sh`)
- DON'T treat `test/live-test.sh` as in-repo unit tests — it hits env-specific LVT URLs/Vault paths

## Key Files

- `Chart.yaml` — chart metadata and deps
- `values.yaml` — auth, DB, RabbitMQ, GUI, vaultAgent, updateStrategy
- `templates/db-migrate-hook.yaml` — migrate Job
- `templates/deployment.yaml` — main update-server Deployment
- `lint.sh` / `test/render-test.sh` — local lint and golden render
