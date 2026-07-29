## Context

Jekyll site for Eclipse IoT Packages docs and the published Helm chart repository content at eclipse.dev/packages.

## Tech

- Jekyll `~> 4` + `jekyll-contentblocks` (`Gemfile`)
- Config: `_config.yml` (`baseurl: /packages`, collections `packages` / `questions`)
- Custom tags in `_plugins/`; package pages under `_packages/`

## Architecture

Content in Markdown/HTML → `jekyll build` → `_site/`. Jenkins (`.jenkins/Jenkinsfile`) builds with the Jekyll container, then on `master` copies into `eclipse/packages-website` and merges newly packaged Helm charts into `charts/`.

## Patterns

- DO serve locally via Docker as in `README.md` (port 4000)
- DO keep chart/package docs aligned with folders under `charts/` and `packages/` (`contribute.md`)
- DON'T edit published chart tarballs here — Jenkins merges from packaged chart artifacts
- DON'T change `baseurl` without coordinating website deploy paths (`_config.yml`)

## Key Files

- `README.md` — local Docker serve command
- `_config.yml` — site URL/baseurl/plugins
- `index.md` — landing page
- `contribute.md` — chart requirements for contributors
- `_packages/cloud2edge/` / `_packages/telemetry-e2e/` — package documentation
