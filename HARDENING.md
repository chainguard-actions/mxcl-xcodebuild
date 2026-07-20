<!-- markdownlint-disable -->

# Hardening Report: mxcl--xcodebuild/v3.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mxcl--xcodebuild/v3.5.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file checks.yml has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/checks.yml:1`

### unpinned-uses (severity: high)

Multiple `uses:` references in checks.yml use mutable tag-based refs instead of pinned 40-character SHA commits, making the workflow vulnerable to supply-chain attacks if the referenced tags are moved or compromised. Failing references include: actions/checkout@v4 (appears on many steps) and mxcl/get-swift-version@v2.

Locations:

- `.github/workflows/checks.yml:20`
- `.github/workflows/checks.yml:28`
- `.github/workflows/checks.yml:36`
- `.github/workflows/checks.yml:50`
- `.github/workflows/checks.yml:64`
- `.github/workflows/checks.yml:76`
- `.github/workflows/checks.yml:88`
- `.github/workflows/checks.yml:100`
- `.github/workflows/checks.yml:112`
- `.github/workflows/checks.yml:125`
- `.github/workflows/checks.yml:148`
- `.github/workflows/checks.yml:161`
- `.github/workflows/checks.yml:178`
- `.github/workflows/checks.yml:196`
- `.github/workflows/checks.yml:198`
- `.github/workflows/checks.yml:218`
- `.github/workflows/checks.yml:248`
- `.github/workflows/checks.yml:265`
- `.github/workflows/checks.yml:267`
- `.github/workflows/checks.yml:271`

### unpinned-uses (severity: high)

Both `uses:` references in vx-tagger.yml use mutable tag-based refs instead of pinned 40-character SHA commits: actions/checkout@v4 and fischerscode/tagger@v0. This is particularly risky as these tags can be silently moved to point to malicious commits.

Locations:

- `.github/workflows/vx-tagger.yml:14`
- `.github/workflows/vx-tagger.yml:15`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

1. Added `permissions: {}` top-level block to checks.yml to enforce least-privilege (vx-tagger.yml already had explicit permissions). 2. Pinned all unpinned action references in checks.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 (14 occurrences) and mxcl/get-swift-version@v2 → @fcfda7d72666658367ddd59503a19bc70c63420d (3 occurrences). 3. Pinned both unpinned action references in vx-tagger.yml: actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 and fischerscode/tagger@v0 → @5ca3fa63ce3003fb7183cae547644b29f3b632be. All SHAs were resolved via lookup_action_sha.

