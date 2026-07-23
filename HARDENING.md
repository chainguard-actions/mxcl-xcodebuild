<!-- markdownlint-disable -->

# Hardening Report: mxcl--xcodebuild/v3.5.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mxcl--xcodebuild/v3.5.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in checks.yml and vx-tagger.yml are pinned to mutable tags or branch names rather than immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action is compromised or the tag is moved. Failing references in checks.yml: `actions/checkout@v4` (used in every job), `mxcl/get-swift-version@v2` (used in swift and verify-dot-swift-version jobs). Failing references in vx-tagger.yml: `actions/checkout@v4`, `fischerscode/tagger@v0`.

Locations:

- `.github/workflows/checks.yml:20`
- `.github/workflows/checks.yml:27`
- `.github/workflows/checks.yml:35`
- `.github/workflows/checks.yml:52`
- `.github/workflows/checks.yml:63`
- `.github/workflows/checks.yml:73`
- `.github/workflows/checks.yml:83`
- `.github/workflows/checks.yml:93`
- `.github/workflows/checks.yml:103`
- `.github/workflows/checks.yml:113`
- `.github/workflows/checks.yml:127`
- `.github/workflows/checks.yml:140`
- `.github/workflows/checks.yml:154`
- `.github/workflows/checks.yml:168`
- `.github/workflows/checks.yml:175`
- `.github/workflows/checks.yml:183`
- `.github/workflows/checks.yml:196`
- `.github/workflows/checks.yml:209`
- `.github/workflows/checks.yml:213`
- `.github/workflows/checks.yml:220`
- `.github/workflows/vx-tagger.yml:13`
- `.github/workflows/vx-tagger.yml:14`

### missing-permissions (severity: medium)

The workflow file checks.yml has no top-level `permissions:` key and none of its jobs define job-level `permissions:` blocks. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents and packages). All jobs in this workflow should have explicit minimal permissions defined.

Locations:

- `.github/workflows/checks.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references in checks.yml and vx-tagger.yml by replacing mutable tags with full 40-character commit SHAs: actions/checkout@v4 → SHA 11d5960a..., mxcl/get-swift-version@v2 → SHA fcfda7d7..., fischerscode/tagger@v0 → SHA 5ca3fa63.... Added top-level `permissions: contents: read` block to checks.yml to address the missing-permissions finding. The vx-tagger.yml already had `permissions: contents: write` which is appropriate for its tag-creation purpose.

