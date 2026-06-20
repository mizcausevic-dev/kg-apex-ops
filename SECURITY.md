# Security posture

`kg-apex-ops` is a public repository that runs scheduled, read-only checks against the private `kineticgain-com-apex` source.

## Credential inventory

By deliberate design, this repo holds:

- **Zero production credentials.** No Hostinger SSH key, no FTP host or user, no deploy-to-live access.
- **Zero signing keys.** The `kg-2026-06` attestation signing key is not present and is never to be added here.
- **One secret only: `APEX_DEPLOY_KEY`**, a **read-only** deploy key on `kineticgain-com-apex`. It can clone the source. It cannot write to the repo and cannot reach the live site.

## Why

This repo is public so its scheduled jobs use free Actions minutes. A public repo is the wrong place for a trust-root signing key or live-server credentials, so none live here. The blast radius of this repo is exactly "read the apex source," nothing more.

## Changing this posture

Any change that adds a production credential or a signing key to this repo requires a **documented decision** recorded before the change: the trade-off, the alternative considered, and the key-rotation plan. The default answer is no. Automate anything that needs those keys on a self-hosted runner on the Hostinger box instead.

## Workflow safety

- Triggers are `schedule` (cron) and `workflow_dispatch` only. There is no `pull_request` trigger, so a fork pull request never runs with this repo's secret.
- `GITHUB_TOKEN` permissions are minimized to `contents: read` and `issues: write`.
