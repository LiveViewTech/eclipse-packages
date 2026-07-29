## Context

Helm chart for Eclipse Hono: protocol adapters, auth, device registry, command router, and messaging (Kafka by default; AMQP profile available).

## Tech

- Helm chart `hono` (`Chart.yaml` appVersion)
- Optional deps: prometheus, grafana, mongodb, kafka (conditions in `Chart.yaml`)
- Example/supporting templates: Artemis, dispatch router, Jaeger, Grafana dashboards under `templates/`
- CI value overlays in `ci/`; deploy profiles as `profile*-values.yaml`

## Architecture

Adapters (MQTT/HTTP/AMQP/CoAP/LoRa) talk to devices; auth + device registry + command router form the control plane; messaging network (Kafka or AMQP example stack) carries telemetry/commands. Monitoring charts are optional via values/profiles.

## Patterns

- DO apply profiles with `helm install ... -f profileAmqpMessaging-values.yaml` (and siblings) — documented in `README.md`
- DO use `ci/*-values.yaml` as references for registry/messaging/monitoring variants
- DON'T enable every optional stack by default — resource cost is high (`values.yaml`, `profileNoMonitoring-values.yaml`)
- DON'T hand-edit example certs lightly — README calls out recreating expired demo certificates

## Key Files

- `Chart.yaml` — versions and conditional dependencies
- `values.yaml` — primary knobs (adapters, messaging, probes, examples)
- `profileAmqpMessaging-values.yaml` / `profileOpenshift-values.yaml` / `profileJaegerBackend-values.yaml`
- `README.md` — install/verify flows
- `example/` — sample tenants, devices, cert helpers
