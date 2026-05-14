# Hardening Report: gradle--gradle-build-action/v3.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gradle--gradle-build-action/v3.3.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses `gradle/actions/setup-gradle@v3.3.2`, which is pinned to a mutable version tag (`v3.3.2`) rather than an immutable 40-character commit SHA. If the tag is moved (intentionally or via a supply-chain attack), the action will silently execute different code. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.3.2`.

Locations:

- `action.yml:229`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced mutable tag `gradle/actions/setup-gradle@v3.3.2` with pinned commit SHA `gradle/actions/setup-gradle@db19848a5fa7950289d3668fb053140cf3028d43 # v3.3.2` in action.yml at line 229. SHA was resolved via lookup_action_sha.

