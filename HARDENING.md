<!-- markdownlint-disable -->

# Hardening Report: JohnnyMorganz--stylua-action/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **JohnnyMorganz--stylua-action/v4.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference external actions using mutable tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a different commit. Affected references:
- `.github/workflows/test.yml`: `actions/checkout@v4` (used 4 times)
- `.github/workflows/update-tags.yml`: `nowactions/update-majorver@v1`
Pin each reference to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:22`
- `.github/workflows/test.yml:27`
- `.github/workflows/test.yml:35`
- `.github/workflows/update-tags.yml:12`

### missing-permissions (severity: medium)

Neither `.github/workflows/test.yml` nor `.github/workflows/update-tags.yml` declares a top-level `permissions:` block, and none of their individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows inherit the repository's default token permissions (often `write-all`), violating the principle of least privilege. Add a top-level `permissions: {}` (or specific minimal scopes) to each workflow.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update-tags.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references: pinned actions/checkout@v4 to SHA 11d5960a326750d5838078e36cf38b85af677262 (4 occurrences in test.yml) and nowactions/update-majorver@v1 to SHA f2014bbbba95b635e990ce512c5653bd0f4753fb (1 occurrence in update-tags.yml). Added top-level `permissions: {}` to test.yml (no special permissions needed) and `permissions: contents: write` to update-tags.yml (required for the update-majorver action to push updated tags).

