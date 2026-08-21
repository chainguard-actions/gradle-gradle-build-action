<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.0** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved.

In action.yml:
- `gradle/actions/setup-gradle@v3.4.0`

In workflow files (representative — all occurrences use tags):
- `actions/checkout@v4` (appears in nearly every workflow)
- `actions/setup-java@v4` (integ-test-execution.yml, integ-test-inject-develocity.yml, integ-test-detect-java-toolchains.yml, integ-test-provision-gradle-versions.yml)
- `actions/github-script@v7` (integ-test-caching-config.yml, integ-test-execution.yml, integ-test-provision-gradle-versions.yml, integ-test-dependency-graph-failures.yml, integ-test-inject-develocity.yml)

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `action.yml:282`
- `.github/workflows/integ-test-action-inputs.yml:22`
- `.github/workflows/integ-test-caching-config.yml:24`
- `.github/workflows/integ-test-caching-config.yml:60`
- `.github/workflows/integ-test-caching-config.yml:97`
- `.github/workflows/integ-test-execution.yml:28`
- `.github/workflows/integ-test-execution.yml:42`
- `.github/workflows/integ-test-execution.yml:60`
- `.github/workflows/integ-test-provision-gradle-versions.yml:27`
- `.github/workflows/integ-test-inject-develocity.yml:30`
- `.github/workflows/integ-test-detect-java-toolchains.yml:22`
- `.github/workflows/demo-failure-cases.yml:12`
- `.github/workflows/demo-job-summary.yml:14`
- `.github/workflows/demo-pr-build-scan-comment.yml:11`

### script-injection (severity: high)

Several `run:` blocks interpolate GitHub Actions expressions directly into shell commands (sub-rule a), bypassing shell quoting and enabling script injection if the values contain shell metacharacters.

1. `integ-test-caching-config.yml` — `${{matrix.gradle}}` is interpolated directly into a shell argument:
   `run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"`
   (appears twice, in the `cache-disabled` and `seed-build-write-only` jobs)

2. `integ-test-provision-gradle-versions.yml` — same pattern:
   `run: gradle help "-DgradleVersionCheck=${{matrix.gradle}}"`

3. `integ-test-dependency-graph.yml` — `${{ steps.*.outputs.dependency-graph-file }}` is interpolated directly into shell commands inside a multi-line `run:` block, including an unquoted `rm` invocation:
   `echo "gradle-assemble report file: ${{ steps.gradle-assemble.outputs.dependency-graph-file }}"`
   `if [ ! -e "${{ steps.gradle-assemble.outputs.dependency-graph-file }}" ]; then`
   `rm ${{ steps.config-cache-store.outputs.dependency-graph-file }}`

All expressions should be moved to `env:` variables and the env vars double-quoted in the shell script.

Locations:

- `.github/workflows/integ-test-caching-config.yml:77`
- `.github/workflows/integ-test-caching-config.yml:113`
- `.github/workflows/integ-test-provision-gradle-versions.yml:89`
- `.github/workflows/integ-test-dependency-graph.yml:72`
- `.github/workflows/integ-test-dependency-graph.yml:100`

### missing-permissions (severity: medium)

The following workflow files have no top-level `permissions:` key and no job-level `permissions:` key on every job. Without explicit permissions, workflows inherit the repository's default token permissions (often `write` for all scopes), violating the principle of least privilege.

Affected files:
- demo-failure-cases.yml
- demo-job-summary.yml
- integ-test-action-inputs.yml
- integ-test-caching-config.yml
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

Each file should declare a top-level `permissions:` block with the minimal scopes required (e.g. `contents: read`).

Locations:

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

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three finding types across the action files:

1. **unpinned-uses**: Pinned all action references to full SHAs:
   - gradle/actions/setup-gradle@v3.4.0 → @d9336dac04dea2507a617466bc058a3def92b18b in action.yml
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 in all workflow files
   - actions/setup-java@v4 → @cf277c60eb25467037889841efdb72551f06f6c3 in all workflow files
   - actions/github-script@v7 → @f28e40c7f34bde8b3046d885e986cb6290c5673b in all workflow files

2. **script-injection**: Moved all ${{ }} expressions from run: blocks to env: blocks:
   - integ-test-caching-config.yml: matrix.gradle → GRADLE_VERSION_CHECK env var (2 jobs)
   - integ-test-provision-gradle-versions.yml: matrix.gradle → GRADLE_VERSION_CHECK env var
   - integ-test-dependency-graph.yml: step output paths → named env vars, rm command now properly quoted

3. **missing-permissions**: Added `permissions: contents: read` top-level block to all 16 workflow files listed in the finding. The integ-test-dependency-graph.yml already had `permissions: contents: write` which was preserved.

### Iteration 2

**Fixes applied:** missing-permissions

**Notes:**

Fixed missing permissions in three workflow files:

1. `.github/workflows/ci-full-check.yml`: Added top-level `permissions: {}` and `permissions: {}` to all 15 jobs that lacked permissions (action-inputs, caching-config, dependency-graph-failures, execution-with-caching, execution, develocity-injection, provision-gradle-versions, restore-configuration-cache, restore-custom-gradle-home, restore-containerized-gradle-home, restore-gradle-home, restore-java-toolchain, sample-kotlin-dsl, sample-gradle-plugin, toolchain-detection). The `dependency-graph` job's existing `contents: write` permission was preserved.

2. `.github/workflows/ci-quick-check.yml`: Added top-level `permissions: {}` and `permissions: {}` to all 15 jobs that lacked permissions. The `dependency-graph` job's existing `contents: write` permission was preserved.

3. `.github/workflows/integ-test-dependency-graph-failures.yml`: Added top-level `permissions: {}` and `permissions: {}` to the two jobs that lacked permissions (`unsupported-gradle-version-warning` and `unsupported-gradle-version-failure`). The other two jobs already had `contents: read` and were left unchanged.

