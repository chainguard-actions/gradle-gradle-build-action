<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.5.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action uses `gradle/actions/setup-gradle@v3.5.0`, which is pinned to a mutable version tag rather than an immutable full 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit, enabling a supply-chain attack. It should be pinned to a specific SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.5.0`.

Locations:

- `action.yml:278`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.5.0` to its full commit SHA `d9c87d481d55275bb5441eef3fe0e46805f9ef70` in hardened/action/action.yml (line 278), preserving the version tag as a comment for readability.

