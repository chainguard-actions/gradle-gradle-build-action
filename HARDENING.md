<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action uses `gradle/actions/setup-gradle@v3.4.1`, which is pinned to a mutable tag (`v3.4.1`) rather than an immutable 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain attack), the action will silently execute different code. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.4.1`.

Locations:

- `action.yml:265`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.4.1` to the immutable commit SHA `31ae3562f68c96d481c31bc1a8a55cc1be162f83` in hardened/action/action.yml. The original tag is preserved as an inline comment (`# v3.4.1`) for readability.

