<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.2** was hardened automatically. 3 finding(s) were identified and resolved across 3 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml references `gradle/actions/setup-gradle@v3.4.2` using a mutable version tag instead of a full 40-character commit SHA. If the tag is moved or the upstream repository is compromised, the action will silently execute different code.

Locations:

- `action.yml:248`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable version tags (`actions/checkout@v4`, `actions/setup-java@v4`, `actions/github-script@v7`) instead of pinned full commit SHAs. This exposes the workflows to supply-chain attacks if any of these tags are moved or the upstream repositories are compromised.

Locations:

- `.github/workflows/demo-failure-cases.yml:10`
- `.github/workflows/demo-job-summary.yml:13`
- `.github/workflows/demo-pr-build-scan-comment.yml:10`
- `.github/workflows/integ-test-action-inputs.yml:22`
- `.github/workflows/integ-test-caching-config.yml:21`
- `.github/workflows/integ-test-dependency-graph-failures.yml:18`
- `.github/workflows/integ-test-dependency-graph.yml:22`
- `.github/workflows/integ-test-detect-java-toolchains.yml:22`
- `.github/workflows/integ-test-execution-with-caching.yml:21`
- `.github/workflows/integ-test-execution.yml:22`
- `.github/workflows/integ-test-inject-develocity.yml:22`
- `.github/workflows/integ-test-provision-gradle-versions.yml:22`
- `.github/workflows/integ-test-restore-configuration-cache.yml:22`
- `.github/workflows/integ-test-restore-containerized-gradle-home.yml:14`
- `.github/workflows/integ-test-restore-custom-gradle-home.yml:18`
- `.github/workflows/integ-test-restore-gradle-home.yml:22`
- `.github/workflows/integ-test-restore-java-toolchain.yml:22`
- `.github/workflows/integ-test-sample-gradle-plugin.yml:22`
- `.github/workflows/integ-test-sample-kotlin-dsl.yml:22`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` block and at least one job also lacks a `permissions:` block. Without explicit permissions, workflows run with the default (potentially write-all) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci-full-check.yml:1`
- `.github/workflows/ci-quick-check.yml:1`
- `.github/workflows/demo-failure-cases.yml:1`
- `.github/workflows/demo-job-summary.yml:1`
- `.github/workflows/integ-test-action-inputs.yml:1`
- `.github/workflows/integ-test-caching-config.yml:1`
- `.github/workflows/integ-test-dependency-graph-failures.yml:1`
- `.github/workflows/integ-test-detect-java-toolchains.yml:1`
- `.github/workflows/integ-test-execution-with-caching.yml:1`
- `.github/workflows/integ-test-execution.yml:1`
- `.github/workflows/integ-test-inject-develocity.yml:1`
- `.github/workflows/integ-test-provision-gradle-versions.yml:1`
- `.github/workflows/integ-test-restore-configuration-cache.yml:1`
- `.github/workflows/integ-test-restore-containerized-gradle-home.yml:1`
- `.github/workflows/integ-test-restore-custom-gradle-home.yml:1`
- `.github/workflows/integ-test-restore-gradle-home.yml:1`
- `.github/workflows/integ-test-restore-java-toolchain.yml:1`
- `.github/workflows/integ-test-sample-gradle-plugin.yml:1`
- `.github/workflows/integ-test-sample-kotlin-dsl.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings: (1) Pinned gradle/actions/setup-gradle@v3.4.2 to full SHA dbbdc275be76ac10734476cc723d82dfe7ec6eda in action.yml. (2) Pinned all mutable action references in workflow files: actions/checkout@v4 → SHA 11d5960a..., actions/setup-java@v4 → SHA cf277c60..., actions/github-script@v7 → SHA f28e40c7... (3) Added 'permissions: {}' top-level blocks to 19 workflow files that were missing them; 2 files (demo-pr-build-scan-comment.yml and integ-test-dependency-graph.yml) already had appropriate permissions blocks and were left unchanged. README.md references were intentionally left unpinned as they are documentation only.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed 3 script injection occurrences across 2 workflow files:
1. `.github/workflows/integ-test-caching-config.yml` (cache-disabled job, line ~77): moved `${{matrix.gradle}}` into `env: GRADLE_VER: ${{ matrix.gradle }}` and changed run to use `$GRADLE_VER`.
2. `.github/workflows/integ-test-caching-config.yml` (cache-disabled-pre-existing-gradle-home job, line ~100): same fix.
3. `.github/workflows/integ-test-provision-gradle-versions.yml` (gradle-versions job, line ~100): same fix.
In all cases, the matrix.gradle value is now passed via an environment variable rather than being template-substituted directly into the shell command string, eliminating the script injection risk.

### Iteration 3

**Fixes applied:** script-injection

**Notes:**

Fixed two script injection vulnerabilities in .github/workflows/integ-test-dependency-graph.yml:

1. 'Check generated dependency graphs' step (job: multiple-builds): Moved three ${{ steps.*.outputs.dependency-graph-file }} expressions into an env: block as ASSEMBLE_GRAPH_FILE, BUILD_GRAPH_FILE, and BUILD_AGAIN_GRAPH_FILE. Updated the run: block to reference these as quoted shell variables.

2. 'Check and delete generated dependency graph' step (job: config-cache): Moved ${{ steps.config-cache-store.outputs.dependency-graph-file }} into an env: block as CONFIG_CACHE_GRAPH_FILE. Updated the run: block to reference it as a quoted shell variable (also added quotes around the rm argument for safety).

