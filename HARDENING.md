<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.5.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action delegates to `gradle/actions/setup-gradle@v3.5.0`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. If the tag is moved (e.g. by a compromised upstream maintainer), the action will silently execute different code. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.5.0`.

Locations:

- `action.yml:244`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced `gradle/actions/setup-gradle@v3.5.0` with `gradle/actions/setup-gradle@d9c87d481d55275bb5441eef3fe0e46805f9ef70 # v3.5.0` in hardened/action/action.yml at line 244. The SHA was resolved via lookup_action_sha for the v3.5.0 tag.

