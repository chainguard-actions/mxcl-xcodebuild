<!-- markdownlint-disable -->

# Hardening Report: mxcl--xcodebuild/v3.4.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mxcl--xcodebuild/v3.4.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag or version refs instead of pinned 40-character SHA commits, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

In .github/workflows/checks.yml:
- `uses: actions/checkout@v4` (appears ~18 times)
- `uses: mxcl/get-swift-version@v2` (appears 2 times)

In .github/workflows/vx-tagger.yml:
- `uses: actions/checkout@v4`
- `uses: fischerscode/tagger@v0`

All of these should be pinned to their full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/checks.yml:20`
- `.github/workflows/vx-tagger.yml:16`

### missing-permissions (severity: medium)

The workflow file .github/workflows/checks.yml has no top-level `permissions:` key and none of its jobs define a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions (which may be `write-all` for older repositories or permissive org defaults), granting the GITHUB_TOKEN broader access than necessary. A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be declared at the top level.

Locations:

- `.github/workflows/checks.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned `uses:` references by resolving them to full 40-character commit SHAs: `actions/checkout@v4` → SHA `11d5960a...` (pinned in both checks.yml and vx-tagger.yml), `mxcl/get-swift-version@v2` → SHA `fcfda7d7...` (3 occurrences in checks.yml), `fischerscode/tagger@v0` → SHA `5ca3fa63...` (in vx-tagger.yml). Added `permissions: contents: read` top-level block to checks.yml to satisfy the missing-permissions finding. The vx-tagger.yml already had an appropriate `permissions: contents: write` block for its release-tagging purpose.

