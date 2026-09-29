<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action delegates to `gradle/actions/setup-gradle@v3.4.0`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. If the tag is moved (e.g. by a supply-chain compromise of the upstream repository), the action will silently execute different code. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-hex-char-sha> # v3.4.0`.

Locations:

- `action.yml:282`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.4.0` to its full commit SHA `d9336dac04dea2507a617466bc058a3def92b18b` in hardened/action/action.yml (line 282), preserving the version tag as a comment for readability.

