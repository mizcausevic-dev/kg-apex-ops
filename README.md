# kg-apex-ops

Scheduled operations for the **private** [`kineticgain-com-apex`](https://github.com/mizcausevic-dev/kineticgain-com-apex) repository, run from a public repo so the jobs use GitHub's free Actions minutes for public repositories.

## What runs here

- **`copy-gate-nightly`** (daily, 07:23 UTC): clones the apex source over SSH using a **read-only** deploy key and runs the copy-standard gate, `python scripts/copy_gate.py --block` and `--explain-residual`. If the gate fails, it files (or updates) a tracking issue on this repo with the output.

## What does NOT run here

- **No attestation re-signing.** The `kg-2026-06` signing key is **not present in this repo and never will be.** Re-signing `/.well-known/kg-verify.json` is done locally and manually for now. The verifier's 30-day OK window (amber `VERIFIED · STALE` after that, which never reds the badge) makes a manual cadence operationally fine. When re-sign is automated, it will run on a self-hosted runner on the Hostinger box, not in a public repo.
- **No production access.** No Hostinger SSH key, no FTP credentials, no deploy-to-live path.

## Access and secrets

This repo holds **read-only** access to the apex source and **zero** production credentials. Its only secret is `APEX_DEPLOY_KEY`, a read-only deploy key on the apex repo. See [`SECURITY.md`](SECURITY.md).
