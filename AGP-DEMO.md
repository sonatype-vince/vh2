# Agent P Demo Template

This repo is a stripped fork of [`juice-shop/juice-shop`](https://github.com/juice-shop/juice-shop) for demoing **Sonatype Agent P**. Upstream workflows that auto-close bot PRs (`pr-compliance.yml`) or run heavy failing CI (`ci.yml`, `codeql`, `zap_scan`) have been removed so Agent P can operate cleanly. Only `.github/workflows/agp-workflow.yml` remains.

## How to use

1. **Fork this repo** into your account or org.
2. **Install the Sonatype Guide GitHub App** on the fork (via the Guide onboarding link — the app opens PR #1 adding `agp-workflow.yml`).
3. **Wait** for the first daily Agent P run — or trigger it manually from the Actions tab (`Sonatype Guide - Agent P` → *Run workflow*).
4. **Import your fork into the SBOM visualiser demo**: paste `owner/repo` into the `IMPORT REPO` field.

Agent P will open PRs against your fork for every fix group it can apply. In the visualiser you'll see them animate against the real Juice Shop dependency graph.

## What was removed vs upstream

Removed `.github/workflows/` files that interfere with Agent P PRs:

- `pr-compliance.yml` — auto-closes any PR that doesn't fill in juice-shop's specific compliance template
- `stale.yml` — closes PRs stalled for 14 days
- `ci.yml` — heavy E2E + build tests; broke on every AGP version bump
- `codeql-analysis.yml`, `zap_scan.yml`, `frontend-bundle-analysis.yml` — security/perf scans that would fail on AGP PRs
- `release.yml`, `image_actions.yml`, `lint-fixer.yml`, `rebase.yml`, `lock.yml` — release-management workflows not needed for the demo
- `update-challenges-*`, `update-news-*` — juice-shop-specific content-refresh workflows

Removed `.github/PULL_REQUEST_TEMPLATE.md` — `pr-compliance.yml` was checking its checkboxes; even with that workflow gone the template was confusing for AGP PRs.

Kept:

- `.github/workflows/agp-workflow.yml` — Sonatype Guide's own workflow that opens the fix PRs

## Refreshing from upstream

To pull juice-shop upstream changes without re-introducing the stripped workflows:

```bash
git remote add upstream https://github.com/juice-shop/juice-shop.git
git fetch upstream
git merge upstream/master --no-commit
git checkout HEAD -- .github/workflows .github/PULL_REQUEST_TEMPLATE.md
git commit -m "Sync from upstream; keep agp-only workflows"
git push
```
