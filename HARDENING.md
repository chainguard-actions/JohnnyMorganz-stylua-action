<!-- markdownlint-disable -->

# Hardening Report: JohnnyMorganz--stylua-action/v4.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **JohnnyMorganz--stylua-action/v4.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references are pinned to mutable tags instead of immutable 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the tag is moved. In .github/workflows/test.yml: `actions/checkout@v4` is used in every job (lines 13, 25, 31, 39, 47). In .github/workflows/update-tags.yml: `nowactions/update-majorver@v1` (line 13). All should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:25`
- `.github/workflows/test.yml:31`
- `.github/workflows/test.yml:39`
- `.github/workflows/test.yml:47`
- `.github/workflows/update-tags.yml:13`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and no individual job defines its own `permissions:` block. Without explicit permissions, workflows run with the default repository permissions (which can be `write-all` on some repositories), granting unnecessarily broad access. Both files should declare minimal required permissions (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. `.github/workflows/test.yml`:
   - Pinned all 5 `actions/checkout@v4` references to `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
   - Added top-level `permissions: contents: read` block (minimal permissions needed for checkout and testing)

2. `.github/workflows/update-tags.yml`:
   - Pinned `nowactions/update-majorver@v1` to `nowactions/update-majorver@f2014bbbba95b635e990ce512c5653bd0f4753fb # v1`
   - Added top-level `permissions: contents: write` block (write access is required for the update-majorver action to push updated major version tags)

