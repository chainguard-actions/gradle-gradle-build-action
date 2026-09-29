<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.4.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action step uses a mutable version tag reference instead of a pinned full-length commit SHA. `uses: gradle/actions/setup-gradle@v3.4.1` should be replaced with a full 40-character hex commit SHA (e.g. `uses: gradle/actions/setup-gradle@<40-char-sha> # v3.4.1`) to prevent supply-chain attacks where the tag is silently moved to a different, potentially malicious commit.

Locations:

- `action.yml:248`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced `uses: gradle/actions/setup-gradle@v3.4.1` with `uses: gradle/actions/setup-gradle@31ae3562f68c96d481c31bc1a8a55cc1be162f83 # v3.4.1` in hardened/action/action.yml line 248. The SHA was resolved via lookup_action_sha for the v3.4.1 tag.

