## Context

End-to-end telemetry package covering sensor/gateway/cloud tiers with Drogue Cloud, Strimzi Kafka, Ditto, and Streamsheets (example-oriented).

## Tech

- Helm package `telemetry-e2e` (`Chart.yaml`)
- Deps: strimzi-kafka-operator, drogue-cloud-common/core, ditto (eclipse packages charts), streamsheets
- Extra dirs: `iofog/`, `crds/`, `post-install/`, `ci/init.sh`
- README still marked “TODO: Move to proper documentation”

## Architecture

Helm installs Kafka operator + Drogue Cloud + Ditto + Streamsheets; templates under `templates/` add ingress/Ditto/Drogue/Kafka/Keycloak glue; post-install JSON defines connections/devices. Local Minikube flow documented in `README.md`.

## Patterns

- DO follow Minikube + `DOMAIN` setup in `README.md` before `helm upgrade --install`
- DO run `ci/init.sh` path via `.github/chart-ci-init.sh` when CI includes this chart (currently excluded)
- DON'T expect `ct lint/install` to cover this package (`.github/ct.yaml` `excluded-charts`)
- DON'T treat README Kura/ioFog sections as fully automated in-cluster installs

## Key Files

- `Chart.yaml` — dependency set and package description
- `values.yaml` — package defaults
- `README.md` — Minikube deploy and demo commands
- `ci/init.sh` — CI init hook
- `post-install/` — connection/device definitions
