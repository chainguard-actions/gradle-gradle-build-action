<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml file references `gradle/actions/setup-gradle@v3.4.2`, which uses a mutable version tag (`v3.4.2`) instead of a pinned 40-character commit SHA. This means the action could silently change if the tag is moved, enabling a supply-chain attack. It should be pinned to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.4.2`.

Locations:

- `action.yml:264`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.4.2` to `gradle/actions/setup-gradle@dbbdc275be76ac10734476cc723d82dfe7ec6eda # v3.4.2` in hardened/action/action.yml. The SHA was resolved via lookup_action_sha for the v3.4.2 tag.

