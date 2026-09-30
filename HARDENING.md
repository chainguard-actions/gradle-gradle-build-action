<!-- markdownlint-disable -->

# Hardening Report: gradle--gradle-build-action/v3.3.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **gradle--gradle-build-action/v3.3.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The composite action delegates to `gradle/actions/setup-gradle@v3.3.2`, which is pinned to a mutable version tag (`v3.3.2`) rather than an immutable 40-character commit SHA. If the tag is moved or the upstream repository is compromised, the action will silently execute different code. Pin to a full SHA, e.g. `gradle/actions/setup-gradle@<40-char-sha> # v3.3.2`.

Locations:

- `action.yml:196`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned `gradle/actions/setup-gradle@v3.3.2` to the full commit SHA `db19848a5fa7950289d3668fb053140cf3028d43` in hardened/action/action.yml (line 196). The original tag is preserved as a comment: `# v3.3.2`.

