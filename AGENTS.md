## Purpose

Eclipse IoT Packages ships Helm charts and composed packages so Eclipse IoT stacks (Hono, Ditto, hawkBit, end-to-end scenarios) deploy on Kubernetes, with a Jekyll site that documents and publishes them.

## Project Snapshot

- Type: multi-component Helm + Jekyll repo
- Stack: Helm, Kubernetes, Bash, Jekyll/Ruby
- Charts: `charts/hawkbit`, `charts/hono`, `charts/ditto` (deprecated locally)
- Packages: `packages/cloud2edge`, `packages/telemetry-e2e`
- Site: `homepage/`
- Model: `.claude/repository-model.yaml`

## Commands

```bash
# Chart CI (from repo root; needs Helm, ct, python as in .github/workflows/ci.yaml)
ct lint --config .github/ct.yaml
.github/kubeval.sh
ct install --config .github/ct.yaml

# hawkBit chart local lint
(cd charts/hawkbit && ./lint.sh)

# hawkBit golden render (needs external ArgoCD values; env: devops|int)
(cd charts/hawkbit && ./test/render-test.sh int)

# Homepage local serve
cd homepage && JEKYLL_VERSION=4 docker run --rm --volume="$PWD:/srv/jekyll:z" --volume="$PWD/vendor/bundle:/usr/local/bundle:z" -eJEKYLL_UID=$UID -p 4000:4000 -it docker.io/jekyll/jekyll:$JEKYLL_VERSION jekyll serve
```

Install examples (cluster + `eclipse-iot` helm repo required):

```bash
helm install eclipse-hawkbit eclipse-iot/hawkbit
helm install eclipse-hono eclipse-iot/hono -n hono --wait
```

## Conventions

- Eclipse ECA + `git commit -s`; prefer `[#issue]` commit prefixes (`CONTRIBUTING.md`)
- New files need EPL-2.0 license headers (`CONTRIBUTING.md`)
- Chart layout: `charts/<project>/` for single projects, `packages/<name>/` for multi-project suites (`homepage/contribute.md`)
- CI only on `charts/**` and `packages/**` (`.github/workflows/ci.yaml`)
- **Ask before editing** `hawkbit_resources.yaml` (fork ops registry; see that file’s header)

## Directory Map

- `charts/hawkbit/` → hawkBit update-server chart (optional `vaultAgent` sidecar hooks in `values.yaml`)
- `charts/hono/` → Hono chart; deploy overlays in `profile*-values.yaml` and `ci/*-values.yaml`
- `charts/ditto/` → deprecated local chart (see Gotchas)
- `packages/cloud2edge/` → Hono+Ditto demo package; post-install Jobs wire demo tenant/device
- `packages/telemetry-e2e/` → Drogue/Kafka/Ditto/Streamsheets e2e package
- `homepage/` → see `homepage/AGENTS.md`

## Gotchas

- **hawkBit upgrades**: pre-install/pre-upgrade DB migrate Job + `updateStrategy: Recreate` (`charts/hawkbit/templates/db-migrate-hook.yaml`, `charts/hawkbit/values.yaml`)
- **Demo credentials** in values are not production-safe (`charts/hawkbit/values.yaml`, `packages/cloud2edge/values.yaml`)
- **`cloud2edge` and `telemetry-e2e` are excluded** from chart-testing (`.github/ct.yaml`)
- hawkBit `test/render-test.sh` / `test/live-test.sh` depend on out-of-repo ArgoCD values and LVT hosts
- Local `charts/ditto` is deprecated; cloud2edge pulls Ditto from OCI (`packages/cloud2edge/Chart.yaml`); Jenkins still packages `charts/*/Chart.yaml` (`.jenkins/Jenkinsfile`)
