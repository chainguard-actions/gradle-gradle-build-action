# Hardening Report: gradle--gradle-build-action/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `ff50f15e4b79bfbf764dafdfd2579175a6ea9771`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **gradle--gradle-build-action/v3.5.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses a mutable version tag reference (`gradle/actions/setup-gradle@v3.5.0`) instead of a pinned 40-character SHA commit hash. This means the action could be silently updated or compromised without the consuming workflow's knowledge, enabling supply-chain attacks.

Locations:

- `action.yml:285`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced mutable tag reference `gradle/actions/setup-gradle@v3.5.0` with pinned SHA `gradle/actions/setup-gradle@d9c87d481d55275bb5441eef3fe0e46805f9ef70 # v3.5.0` in action.yml at the composite action step.

