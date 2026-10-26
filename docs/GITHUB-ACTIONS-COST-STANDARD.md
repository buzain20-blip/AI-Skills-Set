# Standard GitHub Actions cost policy for private repositories

## Objective
Keep private repository CI/build minutes sustainable, without hiding real failures or skipping checks that protect releases and user data. Apply to ACE, ORIN Beta, Salawatak, and future projects.

### Baseline rules
1. **Default CI on Pull Requests and protected branch changes only.** Feature branch pushes without PR should not start duplicate long runs; enable PR checks for features. Do not stop required branch protection checks without updating protection rules.
2. **Relevant path filters.** Skip CI for documentation-only edits and unrelated packages, but ALWAYS run tests when workflow, build files, dependency lockfiles, source or test code change. GitHub workflow files themselves must be in path filters.
3. **Concurrency:** For ordinary CI use one group per workflow and ref/PR and `cancel-in-progress: true`. For releases, production deployment, irreversible steps and owner requests use `cancel-in-progress: false` and a separate concurrency group.
4. **Build once per useful change.** Bundle related edits into a feature branch, inspect with static checks, push a coherent commit, then open/refresh one PR. Avoid repeatedly changing the workflow YAML just to rerun.
5. **Dependency caching:** Java/Gradle via `gradle/actions/setup-gradle`; Node via `actions/setup-node` with npm cache; .NET/Python use dependency-specific caches only where correct. Do not cache credentials, signed keystores, private datasets, or user data.
6. **OS selection:** Prefer Linux for platform-neutral analysis/lint; reserve Windows runners for Windows-native requirements, Revit-related managed tests, and installer/package checks. Do not claim Linux jobs validate native Revit.
7. **Fast gating:** format/lint/compile and small unit tests first; run expensive emulator/native/full regression when code touches their area, on release candidates or manual dispatch. Never skip critical test suites merely to save budget.
8. **Retention:** Keep transient artifacts only when useful, typically 3–7 days for debug; signed production artifacts should be stored in immutable Releases with certificate verification. Avoid uploading every debug APK for every PR.
9. **Release:** manual release or gated automatic release only on reviewed, successfully tested merge; never publish from a failed/cancelled CI job. Original Android signing key stays only in secret storage; certificate pinning prevents mismatched upgrade signing.
10. **Limits & monitoring:** set GitHub Actions-specific metered budget with **Stop usage when budget limit is reached**, and enable 75/90/100% alerts. Budget is not prepaid credit. Plan free minutes reset at the start of the billing cycle; account total is shared across private repos.
11. **Never use billing tricks:** do not make repos public solely for free minutes if they contain private source, personal information, credentials or company work. Do not bypass limits.
12. **Verification:** record workflow commit, observed CI status, minutes/runner types, skipped jobs for path-only changes and signed release evidence before marking change complete.

## Reusable template

```yaml
on:
  pull_request:
    paths:
      - 'src/**'
      - 'tests/**'
      - 'build/**'
      - 'package-lock.json'
      - '.github/workflows/ci.yml'
  push:
    branches: [main]
    paths:
      - 'src/**'
      - 'tests/**'
      - 'build/**'
      - 'package-lock.json'
      - '.github/workflows/ci.yml'
  workflow_dispatch:

concurrency:
  group: ci-${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: ${{ github.event_name != 'workflow_dispatch' }}

permissions:
  contents: read
```

Adapt directories, languages, runner OS and required gates to each project. Do not blindly copy template path filters or dependencies. Branch protection should still require critical checks on relevant code changes.

## Billing

Open https://github.com/settings/billing and navigate to **Budgets and alerts**. A **$2 Actions budget** is a *monthly billable-overage cap*, not a $2 prepaid wallet. First configure a valid permitted payment method in **Payment information**, then set an Actions budget scoped to the intended personal account, enable **Stop usage when budget limit is reached**, and budget alerts. GitHub pricing and UI may change; verify at https://docs.github.com/en/billing/concepts/product-billing/github-actions and https://docs.github.com/en/billing/how-tos/set-up-budgets.

The free allowance on GitHub Free has been documented as **2,000 minutes per month**, reset at the beginning of the billing cycle. Windows and macOS runner minutes cost more than Linux, and costs are charged to the repo owner's account.

## Applied 2026-10-26
- **ORIN-Beta:** source-checks workflow path filtering, PR-based feature validation, cancellation of superseded runs; owner manual workflow remains, Windows required for managed-source validation, native Revit not claimed.
- **Salawatak Android:** documentation-only changes excluded by CI path filters; existing PR concurrency, Gradle cache and explicit signed release preserved.
- **ACE-Ring-Bridge:** existing CI already includes path filters, concurrency cancellation, Gradle cache and manual signed releases.
- **Salawatak Flutter:** no workflow directory currently found; check build-system readiness before creating CI.
