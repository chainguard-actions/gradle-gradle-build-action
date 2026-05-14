# Hardening Report: gradle--gradle-build-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gradle--gradle-build-action/v3.4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses a mutable version tag reference (`@v3.4.1`) instead of a pinned 40-character commit SHA. This means the action could be silently updated or replaced by a supply-chain attacker without the consuming workflow noticing. The failing reference is: `gradle/actions/setup-gradle@v3.4.1`. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.4.1`.

Locations:

- `action.yml:270`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.4.1` to its full commit SHA `31ae3562f68c96d481c31bc1a8a55cc1be162f83` in action.yml (line 270). The original tag is preserved as a comment for readability: `gradle/actions/setup-gradle@31ae3562f68c96d481c31bc1a8a55cc1be162f83 # v3.4.1`.

