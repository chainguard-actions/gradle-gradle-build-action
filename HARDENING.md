<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action uses `gradle/actions/setup-gradle@v3.4.0` — a mutable version tag rather than a pinned 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain compromise), the action will silently execute different code. Pin to a full SHA, e.g. `gradle/actions/setup-gradle@<40-hex-char-sha> # v3.4.0`.

Locations:

- `action.yml:198`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.4.0` to the full commit SHA `d9336dac04dea2507a617466bc058a3def92b18b` in hardened/action/action.yml (line 198). The original tag is preserved as an inline comment for readability.

