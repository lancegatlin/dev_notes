# `workflow_dispatch` + Dry Run for `ci-merge.yml`

## Key Facts: Does it need to be merged first?

**Yes, `workflow_dispatch` changes must be on the default branch to trigger from the GitHub UI.**
GitHub only reads `workflow_dispatch` triggers from the **default branch** (usually `main` or `master`) when using the web UI.

**However**, you can test from a PR branch using the GitHub CLI or REST API — no merge required.

---

## Option 1: Trigger via GitHub CLI (works from any branch)

```bash
gh workflow run ci-merge.yml \
  --ref your-pr-branch \
  --field dry_run=true
```

This bypasses the UI restriction and lets you test your workflow changes from the PR branch **before merging**.

---

## Option 2: Trigger via GitHub REST API (works from any branch)

```bash
curl -X POST \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/repos/ORG/REPO/actions/workflows/ci-merge.yml/dispatches \
  -d '{"ref":"your-pr-branch","inputs":{"dry_run":"true"}}'
```

---

## Option 3: Merge to default branch first (GitHub UI trigger)

If you want the GitHub UI **"Run workflow"** button to appear and work, the workflow with `workflow_dispatch` must be merged to the default branch first.

---

## What to add to `ci-merge.yml`

### 1. Add the `workflow_dispatch` trigger with a `dry_run` input

```yaml
on:
  push:
    branches:
      - main        # keep existing trigger
  workflow_dispatch:
    inputs:
      dry_run:
        description: 'Dry run (skip mutations/destructive steps)'
        required: false
        default: 'false'
        type: choice
        options:
          - 'true'
          - 'false'
```

### 2. Reference `dry_run` in steps — Option A: per-step condition

```yaml
- name: Some mutating step
  if: ${{ inputs.dry_run != 'true' }}
  run: ./deploy-or-mutate.sh
```

### 3. Reference `dry_run` in steps — Option B: environment variable

```yaml
env:
  DRY_RUN: ${{ inputs.dry_run || 'false' }}
```

> The `|| 'false'` fallback ensures `DRY_RUN` is always set even when the workflow
> is triggered by a `push` event (where `inputs` is empty/null).

---

## Summary Table

| Trigger Method | Branch Restriction | Requires Merge to Default? |
|---|---|---|
| GitHub UI ("Run workflow") | ✅ Default branch only | ✅ Yes |
| `gh workflow run --ref <branch>` | ❌ Any branch | ❌ No |
| GitHub REST API (`/dispatches`) | ❌ Any branch | ❌ No |

**Recommendation**: Use `gh workflow run --ref your-pr-branch --field dry_run=true`
to iterate on workflow changes in your PR branch before merging.

---

## Bonus: Shim Pattern — `workflow_dispatch` calling an existing `workflow_call`

If `ci-merge.yml` already uses `on: workflow_call` and you don't want to modify it,
you can create a **shim workflow** that wraps it with `workflow_dispatch`.

### How it works

```
ci-merge-dispatch.yml   ← new shim (workflow_dispatch trigger)
        │
        └──calls──▶  ci-merge.yml  (existing workflow_call trigger, unchanged)
```

### `ci-merge-dispatch.yml` (the shim)

```yaml
name: CI Merge (Manual Dispatch)

on:
  workflow_dispatch:
    inputs:
      dry_run:
        description: 'Dry run (skip mutations/destructive steps)'
        required: false
        default: 'false'
        type: choice
        options:
          - 'true'
          - 'false'

jobs:
  call-ci-merge:
    uses: ./.github/workflows/ci-merge.yml
    with:
      dry_run: ${{ inputs.dry_run == 'true' }}
    secrets: inherit   # pass through all secrets from the caller
```

> `secrets: inherit` forwards all secrets automatically so you don't have to
> re-declare them. Requires GitHub Actions runner 2025+/GHES 3.8+.

### `ci-merge.yml` — add `dry_run` input to the existing `workflow_call`

```yaml
on:
  workflow_call:
    inputs:
      dry_run:
        description: 'Dry run mode'
        required: false
        default: false
        type: boolean
```

Then gate mutating steps the same way:

```yaml
- name: Some mutating step
  if: ${{ !inputs.dry_run }}
  run: ./deploy-or-mutate.sh
```

### Invoking the shim from a PR branch via CLI

```bash
# Trigger the shim on your PR branch (shim file must exist on that branch)
gh workflow run ci-merge-dispatch.yml \
  --ref your-pr-branch \
  --field dry_run=true
```

### Branch rules for the shim pattern

| File | Needs to be on default branch for UI? | CLI (`--ref`) works from PR branch? |
|---|---|---|
| `ci-merge-dispatch.yml` (shim) | ✅ Yes (for UI) | ✅ Yes (if file exists on that branch) |
| `ci-merge.yml` (reusable) | ✅ Yes | ✅ Yes (called via `./.github/workflows/`) |

> **Key insight**: `uses: ./.github/workflows/ci-merge.yml` resolves relative to the
> **caller's ref** (the branch you pass to `--ref`), so both files just need to exist
> on your PR branch — no merge required when using the CLI.

