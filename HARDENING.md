<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses `gradle/actions/setup-gradle@v3.4.1` — a mutable version tag, not a pinned 40-character commit SHA. If the upstream tag is moved or the repository is compromised, the action will silently execute different code. All workflow files also use unpinned tag references: `actions/checkout@v4`, `actions/setup-java@v4`, and `actions/github-script@v7` instead of full SHA digests.

Locations:

- `action.yml:1`
- `.github/workflows/demo-failure-cases.yml:1`
- `.github/workflows/demo-job-summary.yml:1`
- `.github/workflows/demo-pr-build-scan-comment.yml:1`
- `.github/workflows/integ-test-action-inputs.yml:1`
- `.github/workflows/integ-test-caching-config.yml:1`
- `.github/workflows/integ-test-dependency-graph-failures.yml:1`
- `.github/workflows/integ-test-dependency-graph.yml:1`
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

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and at least one job also lacks a job-level `permissions:` key. Without explicit permissions, jobs inherit the default (often `write-all` for classic repos), granting unnecessary access. Files ci-full-check.yml and ci-quick-check.yml have permissions only on the `dependency-graph` job but not on any other jobs and no top-level block. All remaining listed files have no permissions declarations at all.

Locations:

- `.github/workflows/ci-full-check.yml:1`
- `.github/workflows/ci-quick-check.yml:1`
- `.github/workflows/demo-failure-cases.yml:1`
- `.github/workflows/demo-job-summary.yml:1`
- `.github/workflows/integ-test-action-inputs.yml:1`
- `.github/workflows/integ-test-caching-config.yml:1`
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

### script-injection (severity: high)

Rule (a) violation: GitHub Actions expressions are interpolated directly inside `run:` shell command strings, bypassing shell quoting. (1) integ-test-caching-config.yml: two `run:` steps contain `run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"` — the matrix value is injected directly into the shell command. (2) integ-test-provision-gradle-versions.yml: one `run:` step contains the same pattern `run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"`. (3) integ-test-dependency-graph.yml: multiple `run:` steps interpolate `${{ steps.gradle-assemble.outputs.dependency-graph-file }}`, `${{ steps.gradle-build.outputs.dependency-graph-file }}`, `${{ steps.config-cache-store.outputs.dependency-graph-file }}` directly inside shell `echo`, `if [ ! -e ... ]`, and `rm` commands. Any of these values containing shell metacharacters could lead to command injection.

Locations:

- `.github/workflows/integ-test-caching-config.yml:75`
- `.github/workflows/integ-test-caching-config.yml:100`
- `.github/workflows/integ-test-provision-gradle-versions.yml:88`
- `.github/workflows/integ-test-dependency-graph.yml:72`
- `.github/workflows/integ-test-dependency-graph.yml:95`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all three finding types across 21 files:

1. unpinned-uses: Pinned gradle/actions/setup-gradle@v3.4.1 to SHA 31ae3562f68c96d481c31bc1a8a55cc1be162f83 in action.yml. Pinned actions/checkout@v4 (SHA 11d5960a326750d5838078e36cf38b85af677262), actions/setup-java@v4 (SHA cf277c60eb25467037889841efdb72551f06f6c3), and actions/github-script@v7 (SHA f28e40c7f34bde8b3046d885e986cb6290c5673b) in all workflow files.

2. missing-permissions: Added top-level 'permissions: contents: read' to all 18 workflow files that lacked it. The integ-test-dependency-graph.yml already had 'permissions: contents: write' and was preserved.

3. script-injection: In integ-test-caching-config.yml and integ-test-provision-gradle-versions.yml, moved ${{matrix.gradle}} from run: shell commands to env: blocks as GRADLE_VERSION_CHECK. In integ-test-dependency-graph.yml, moved ${{ steps.*.outputs.dependency-graph-file }} expressions from inline shell commands (echo, if [ ! -e ], rm) to env: blocks as named variables (ASSEMBLE_GRAPH_FILE, BUILD_GRAPH_FILE, BUILD_AGAIN_GRAPH_FILE, CONFIG_CACHE_GRAPH_FILE), referenced safely as $VAR in shell.

