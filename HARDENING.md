<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.5.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

action.yml uses `gradle/actions/setup-gradle@v3.5.0` — a mutable version tag instead of a pinned 40-character commit SHA. This exposes the action to supply-chain attacks if the tag is moved.

Locations:

- `action.yml:248`

### unpinned-uses (severity: high)

Multiple workflow files use external actions pinned to mutable version tags instead of full SHA digests. Failing references include: `actions/checkout@v4`, `actions/setup-java@v4`, `actions/github-script@v7`.

Locations:

- `.github/workflows/demo-failure-cases.yml:10`
- `.github/workflows/demo-job-summary.yml:14`
- `.github/workflows/demo-pr-build-scan-comment.yml:9`
- `.github/workflows/integ-test-action-inputs.yml:22`
- `.github/workflows/integ-test-caching-config.yml:22`
- `.github/workflows/integ-test-dependency-graph-failures.yml:18`
- `.github/workflows/integ-test-dependency-graph.yml:22`
- `.github/workflows/integ-test-detect-java-toolchains.yml:24`
- `.github/workflows/integ-test-execution-with-caching.yml:22`
- `.github/workflows/integ-test-execution.yml:30`
- `.github/workflows/integ-test-inject-develocity.yml:36`
- `.github/workflows/integ-test-provision-gradle-versions.yml:30`
- `.github/workflows/integ-test-restore-configuration-cache.yml:28`
- `.github/workflows/integ-test-restore-containerized-gradle-home.yml:16`
- `.github/workflows/integ-test-restore-custom-gradle-home.yml:18`
- `.github/workflows/integ-test-restore-gradle-home.yml:22`
- `.github/workflows/integ-test-restore-java-toolchain.yml:22`
- `.github/workflows/integ-test-sample-gradle-plugin.yml:22`
- `.github/workflows/integ-test-sample-kotlin-dsl.yml:22`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` block and at least one job without a job-level `permissions:` block. Without explicit permissions, workflows inherit the default (potentially write-all) token permissions, violating least-privilege.

Locations:

- `.github/workflows/demo-failure-cases.yml:1`
- `.github/workflows/demo-job-summary.yml:1`
- `.github/workflows/ci-full-check.yml:1`
- `.github/workflows/ci-quick-check.yml:1`
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

### script-injection (severity: high)

Rule (a) violation: `${{ ... }}` expressions are interpolated directly inside `run:` shell command strings. In `integ-test-dependency-graph.yml`, step outputs (`${{ steps.gradle-assemble.outputs.dependency-graph-file }}`, `${{ steps.gradle-build.outputs.dependency-graph-file }}`, `${{ steps.gradle-build-again.outputs.dependency-graph-file }}`, `${{ steps.config-cache-store.outputs.dependency-graph-file }}`) are used directly in shell commands including as arguments to `rm` and in `if [ ! -e "..."]` tests. These values flow through YAML template substitution before the shell sees them.

Locations:

- `.github/workflows/integ-test-dependency-graph.yml:79`
- `.github/workflows/integ-test-dependency-graph.yml:82`
- `.github/workflows/integ-test-dependency-graph.yml:85`
- `.github/workflows/integ-test-dependency-graph.yml:101`
- `.github/workflows/integ-test-dependency-graph.yml:104`

### script-injection (severity: high)

Rule (a) violation: `${{matrix.gradle}}` is interpolated directly inside a `run:` shell command string: `run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"`. Matrix values are workflow-controllable and flow through YAML template substitution before the shell processes them.

Locations:

- `.github/workflows/integ-test-caching-config.yml:72`
- `.github/workflows/integ-test-caching-config.yml:91`
- `.github/workflows/integ-test-provision-gradle-versions.yml:91`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection

**Notes:**

Fixed all findings across action.yml and 21 workflow files:

1. **unpinned-uses (action.yml)**: Pinned `gradle/actions/setup-gradle@v3.5.0` to SHA `d9c87d481d55275bb5441eef3fe0e46805f9ef70`.

2. **unpinned-uses (workflow files)**: Pinned all mutable tags to full commit SHAs:
   - `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262`
   - `actions/setup-java@v4` → `@cf277c60eb25467037889841efdb72551f06f6c3`
   - `actions/github-script@v7` → `@f28e40c7f34bde8b3046d885e986cb6290c5673b`

3. **missing-permissions**: Added top-level `permissions: contents: read` to all workflow files that lacked a permissions block (ci-full-check.yml, ci-quick-check.yml, demo-failure-cases.yml, demo-job-summary.yml, and all integ-test-*.yml files). demo-pr-build-scan-comment.yml already had `permissions: pull-requests: write` which was preserved.

4. **script-injection (integ-test-dependency-graph.yml)**: Moved `${{ steps.*.outputs.dependency-graph-file }}` expressions out of `run:` shell strings into `env:` blocks, then referenced them as `$ASSEMBLE_GRAPH_FILE`, `$BUILD_GRAPH_FILE`, `$BUILD_AGAIN_GRAPH_FILE`, and `$CONFIG_CACHE_GRAPH_FILE` in the shell commands. Also fixed the unquoted `rm` to use `rm "$CONFIG_CACHE_GRAPH_FILE"`.

5. **script-injection (integ-test-caching-config.yml, integ-test-provision-gradle-versions.yml)**: Moved `${{matrix.gradle}}` out of `run:` shell strings into `env: GRADLE_VERSION_CHECK: ${{ matrix.gradle }}` and referenced as `$GRADLE_VERSION_CHECK` in the shell commands.

