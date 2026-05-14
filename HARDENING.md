# Hardening Report: gradle--gradle-build-action/v3.4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gradle--gradle-build-action/v3.4.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses a mutable version tag reference (`gradle/actions/setup-gradle@v3.4.2`) instead of a pinned 40-character SHA commit hash. This means the action could be silently updated or compromised without the consuming workflow noticing, enabling supply-chain attacks.

Locations:

- `action.yml:271`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced mutable tag reference `gradle/actions/setup-gradle@v3.4.2` with pinned SHA `gradle/actions/setup-gradle@dbbdc275be76ac10734476cc723d82dfe7ec6eda # v3.4.2` in action.yml at line 271. The SHA was resolved via lookup_action_sha for the v3.4.2 tag.

