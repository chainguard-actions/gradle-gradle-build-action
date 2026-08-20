<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.3.2** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple action references use mutable tags instead of pinned 40-character commit SHAs, making the action vulnerable to supply-chain attacks if the tag is moved.

In action.yml:
  - uses: gradle/actions/setup-gradle@v3.3.2

In workflow files, every external action reference uses a tag:
  - uses: actions/checkout@v4 (appears in all integ-test-*.yml, demo-*.yml files)
  - uses: actions/github-script@v7 (in integ-test-caching-config.yml, integ-test-provision-gradle-versions.yml)

All of these should be pinned to their full 40-character commit SHA.

Locations:

- `action.yml:196`
- `.github/workflows/demo-failure-cases.yml:10`
- `.github/workflows/demo-job-summary.yml:13`
- `.github/workflows/demo-pr-build-scan-comment.yml:12`
- `.github/workflows/integ-test-action-inputs.yml:20`
- `.github/workflows/integ-test-caching-config.yml:22`
- `.github/workflows/integ-test-dependency-graph-failures.yml:18`
- `.github/workflows/integ-test-dependency-graph.yml:22`
- `.github/workflows/integ-test-detect-java-toolchains.yml:22`
- `.github/workflows/integ-test-execution-with-caching.yml:20`
- `.github/workflows/integ-test-execution.yml:22`
- `.github/workflows/integ-test-inject-develocity.yml:22`
- `.github/workflows/integ-test-provision-gradle-versions.yml:22`
- `.github/workflows/integ-test-restore-configuration-cache.yml:22`
- `.github/workflows/integ-test-restore-containerized-gradle-home.yml:14`
- `.github/workflows/integ-test-restore-custom-gradle-home.yml:22`
- `.github/workflows/integ-test-restore-gradle-home.yml:22`
- `.github/workflows/integ-test-restore-java-toolchain.yml:20`
- `.github/workflows/integ-test-sample-gradle-plugin.yml:20`
- `.github/workflows/integ-test-sample-kotlin-dsl.yml:20`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation inside run: shell commands. The matrix context value ${{matrix.gradle}} is interpolated directly into a shell command string, allowing a workflow caller to inject arbitrary shell content via the matrix value.

Offending lines:
  - run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"  (integ-test-caching-config.yml, two occurrences)
  - run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"  (integ-test-provision-gradle-versions.yml)

Fix: move the value into an env: variable and reference it as a quoted shell variable, e.g. env: GRADLE_VERSION: ${{ matrix.gradle }} then run: gradle help "-DgradleVersionCheck=$GRADLE_VERSION"

Locations:

- `.github/workflows/integ-test-caching-config.yml:74`
- `.github/workflows/integ-test-caching-config.yml:98`
- `.github/workflows/integ-test-provision-gradle-versions.yml:96`

### missing-permissions (severity: medium)

Multiple workflow files have no top-level permissions: key and do not define permissions on every job, meaning jobs run with the default (potentially broad) GITHUB_TOKEN permissions.

Files with no permissions block at all:
  - demo-failure-cases.yml
  - demo-job-summary.yml
  - integ-test-action-inputs.yml
  - integ-test-caching-config.yml
  - integ-test-dependency-graph-failures.yml (has job-level permissions on only some jobs, not all)
  - integ-test-detect-java-toolchains.yml
  - integ-test-execution-with-caching.yml
  - integ-test-execution.yml
  - integ-test-inject-develocity.yml
  - integ-test-provision-gradle-versions.yml
  - integ-test-restore-configuration-cache.yml
  - integ-test-restore-containerized-gradle-home.yml
  - integ-test-restore-custom-gradle-home.yml
  - integ-test-restore-gradle-home.yml
  - integ-test-restore-java-toolchain.yml
  - integ-test-sample-gradle-plugin.yml
  - integ-test-sample-kotlin-dsl.yml

Files with permissions on only one job (not all jobs):
  - ci-full-check.yml (only the dependency-graph job has permissions: contents: write)
  - ci-quick-check.yml (only the dependency-graph job has permissions: contents: write)

All workflow files should declare a top-level permissions: block with the minimum required scopes.

Locations:

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
- `.github/workflows/ci-full-check.yml:1`
- `.github/workflows/ci-quick-check.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings:

1. unpinned-uses: Pinned all action references to full 40-character SHAs:
   - gradle/actions/setup-gradle@v3.3.2 → @db19848a5fa7950289d3668fb053140cf3028d43
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262
   - actions/github-script@v7 → @f28e40c7f34bde8b3046d885e986cb6290c5673b
   - actions/setup-java@v4 → @cf277c60eb25467037889841efdb72551f06f6c3

2. script-injection: Fixed 3 occurrences of ${{matrix.gradle}} in run: shell commands by moving the value to env: GRADLE_VERSION: ${{ matrix.gradle }} and referencing $GRADLE_VERSION in the shell.

3. missing-permissions: Added top-level permissions: contents: read to all 17 workflow files that were missing permissions blocks. Also added permissions: contents: read to ci-full-check.yml and ci-quick-check.yml (which had job-level permissions only on the dependency-graph job). Updated demo-pr-build-scan-comment.yml to include both contents: read and pull-requests: write at top level.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two script injection vulnerabilities in .github/workflows/integ-test-dependency-graph.yml:

1. 'Check generated dependency graphs' step (multiple-builds job): Moved `${{ steps.gradle-assemble.outputs.dependency-graph-file }}`, `${{ steps.gradle-build.outputs.dependency-graph-file }}`, and `${{ steps.gradle-build-again.outputs.dependency-graph-file }}` expressions into an `env:` block as `GRADLE_ASSEMBLE_DEP_GRAPH_FILE`, `GRADLE_BUILD_DEP_GRAPH_FILE`, and `GRADLE_BUILD_AGAIN_DEP_GRAPH_FILE` respectively. All shell references updated to use quoted `"$VAR"` form.

2. 'Check and delete generated dependency graph' step (config-cache job): Moved `${{ steps.config-cache-store.outputs.dependency-graph-file }}` into an `env:` block as `CONFIG_CACHE_STORE_DEP_GRAPH_FILE`. Also fixed the unquoted `rm ${{ ... }}` to use `rm "$CONFIG_CACHE_STORE_DEP_GRAPH_FILE"` for proper quoting.

