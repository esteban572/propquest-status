# PropQuest Status

Public uptime monitoring for the PropQuest platform. Powered by [Upptime](https://upptime.js.org/) — a free, GitHub-Actions-driven status page generator.

This is the **sister repo** that hosts:

- The Upptime configuration (`.upptimerc.yml`)
- The GitHub Actions workflows that run uptime checks every 5 minutes
- The published status page at [status.propquest.ai](https://status.propquest.ai/)

## What this monitors

| Site | URL |
|---|---|
| PropQuest Homepage | https://propquest.ai/ |
| Privacy Policy | https://propquest.ai/privacy-policy |
| Terms of Service | https://propquest.ai/terms-of-service |
| API Health Check | https://api.propquest.ai/up |
| App Login | https://app.propquest.ai/login |
| Cal Booking Page | https://cal.propquest.ai/esteban/demo |

Checks run every 5 minutes from GitHub's runners.

## Adding or removing a monitored site

1. Edit `.upptimerc.yml` — add or remove an entry under `sites:`.
2. Push to `master` (or `main`).
3. The Setup workflow re-bootstraps history files; the next Uptime run picks up the change.

Each entry needs:

```yaml
- name: Friendly Name
  url: https://example.com/
  expectedStatusCodes:
    - 200
    - 301
    - 302
```

## Custom domain — `status.propquest.ai`

This repo is configured with `cname: status.propquest.ai` in `.upptimerc.yml`.

To activate the custom domain:

1. **GitHub Pages** → Settings → Pages → custom domain → `status.propquest.ai`
2. **DNS** (Cloudflare or wherever propquest.ai DNS lives) → add a CNAME record:
   ```
   Type:  CNAME
   Name:  status
   Value: esteban572.github.io
   TTL:   Auto / 300
   Proxy: DNS only (orange cloud OFF on Cloudflare — GitHub Pages handles TLS)
   ```
3. Wait 5–15 minutes for propagation, then check `https://status.propquest.ai/`.

## Where alerts go

**V1 (current):** Upptime opens a GitHub issue in this repo when a check fails. The issue is auto-assigned to `@esteban572`. The issue auto-closes when the check recovers.

**Follow-up (deferred):** wire Slack or Discord notifications by uncommenting the `notifications:` block in `.upptimerc.yml` and adding the relevant secret (`SLACK_WEBHOOK_URL` or `DISCORD_WEBHOOK_URL`) under repo Settings → Secrets and variables → Actions.

## Workflows

| Workflow | Cadence | Purpose |
|---|---|---|
| `uptime.yml` | every 5 min | Run health checks against all sites |
| `response-time.yml` | daily | Update response-time graphs |
| `summary.yml` | daily | Regenerate README status badges |
| `graphs.yml` | daily | Regenerate the uptime graphs |
| `static-site.yml` | hourly | Build + deploy the public status page to GitHub Pages |
| `setup.yml` | on config change | Bootstrap history files when sites change |
| `updates.yml` | daily 03:00 UTC | Pull upstream Upptime template updates |

## Permissions / secrets

The default `GITHUB_TOKEN` is sufficient for everything. If you want commits attributed to a different identity (or hit rate-limit issues), add a fine-grained `GH_PAT` secret with `contents: write` + `issues: write` on this repo.

## Source of truth

Configuration lives in [`esteban572/propquest`](https://github.com/esteban572/propquest) under `tools/upptime/`. Changes to this repo are mirrored back there manually for the moment — if you want a more sophisticated sync, set up a one-way GitHub Action that copies `tools/upptime/` from the main repo on push to main.

## License

MIT — same as the upstream Upptime project.
