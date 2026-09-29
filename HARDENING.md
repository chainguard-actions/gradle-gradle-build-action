<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.3.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses `gradle/actions/setup-gradle@v3.3.2`, which is pinned to a mutable version tag rather than an immutable 40-character commit SHA. A tag can be moved to point to a different (potentially malicious) commit at any time, enabling a supply-chain attack. It should be replaced with a full SHA pin, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.3.2`.

Locations:

- `action.yml:196`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced mutable tag reference `gradle/actions/setup-gradle@v3.3.2` at action.yml line 196 with the full commit SHA pin `gradle/actions/setup-gradle@db19848a5fa7950289d3668fb053140cf3028d43 # v3.3.2`. The SHA was resolved via lookup_action_sha.

