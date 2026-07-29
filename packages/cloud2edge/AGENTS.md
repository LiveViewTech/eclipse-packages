## Context

Package chart composing Eclipse Hono + Ditto into a cloud-to-edge demo, with post-install Jobs that wire demo device credentials and Hono↔Ditto connections.

## Tech

- Helm package `cloud2edge`
- Dependencies: Hono from `https://eclipse.org/packages/charts/`, Ditto from `oci://registry-1.docker.io/eclipse` (`Chart.yaml`)
- Profiles: `profileAmqpMessaging-values.yaml`, `profileTracing-values.yaml`, `profileOpenshift-values.yaml`
- Scripts/JSON under `post-install/`

## Architecture

Parent chart values nest `hono` / Ditto settings; Helm installs subcharts; `templates/post-install-job.yaml` runs `post-install/post-install.sh` to register demo tenant/device and create the Ditto connection to Hono messaging.

## Patterns

- DO apply profiles with `helm install ... -f profileTracing-values.yaml` (`README.md`)
- DO treat passwords in `values.yaml` (`demo-secret`, `verysecret`) as demo-only
- DON'T expect chart-testing CI coverage — listed in `.github/ct.yaml` `excluded-charts`
- DON'T confuse local `charts/ditto` with the OCI Ditto dependency used here

## Key Files

- `Chart.yaml` — package metadata and subchart pins
- `values.yaml` — demo device, Hono/Ditto nested config
- `templates/post-install-job.yaml` — wiring Job
- `post-install/post-install.sh` — connection/device setup
- `README.md` — profiles and release notes (incl. migration warnings)
