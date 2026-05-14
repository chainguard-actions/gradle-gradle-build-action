# Hardening Report: gradle--gradle-build-action/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gradle--gradle-build-action/v3.4.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses a mutable tag reference (`gradle/actions/setup-gradle@v3.4.0`) instead of a pinned 40-character commit SHA. If the tag is moved or the upstream repository is compromised, the action will silently execute different code, enabling a supply-chain attack.

Locations:

- `action.yml:280`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced mutable tag reference `gradle/actions/setup-gradle@v3.4.0` with pinned commit SHA `gradle/actions/setup-gradle@d9336dac04dea2507a617466bc058a3def92b18b # v3.4.0` in action.yml at line 280. The SHA was resolved via lookup_action_sha for the v3.4.0 tag.

