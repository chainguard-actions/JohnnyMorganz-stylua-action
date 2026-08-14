<!-- markdownlint-disable -->

# Hardening Report: JohnnyMorganz--stylua-action/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **JohnnyMorganz--stylua-action/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow steps use action references pinned to mutable tags rather than immutable full-length SHA commits. In test.yml, `actions/checkout@v3` is used three times (tag ref). In update-tags.yml, `nowactions/update-majorver@v1` is used (tag ref). These should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`) to prevent supply-chain attacks via tag mutation.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:24`
- `.github/workflows/test.yml:29`
- `.github/workflows/update-tags.yml:13`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within them declares job-level `permissions:`. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. write access to contents). Both test.yml and update-tags.yml should declare minimal required permissions (e.g. `permissions: read-all` or specific scopes).

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 unpinned action references by resolving them to full 40-character commit SHAs: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 (4 occurrences in test.yml), nowactions/update-majorver@v1 → @f2014bbbba95b635e990ce512c5653bd0f4753fb (1 occurrence in update-tags.yml). Added top-level permissions blocks to both workflow files: test.yml gets 'permissions: {}' (no special permissions needed for build/test), and update-tags.yml gets 'permissions: { contents: write }' (required for the update-majorver action to push/update tags).

