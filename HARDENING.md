<!-- markdownlint-disable -->

# Hardening Report: mxcl--xcodebuild/v3.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mxcl--xcodebuild/v3.3.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in .github/workflows/checks.yml use mutable tag-based refs instead of pinned 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks. Failing references include: `actions/checkout@v4` (appears 18 times across all jobs) and `mxcl/get-swift-version@v2` (appears 3 times in the swift and verify-dot-swift-version jobs).

Locations:

- `.github/workflows/checks.yml:20`
- `.github/workflows/checks.yml:27`
- `.github/workflows/checks.yml:35`
- `.github/workflows/checks.yml:49`
- `.github/workflows/checks.yml:64`
- `.github/workflows/checks.yml:76`
- `.github/workflows/checks.yml:88`
- `.github/workflows/checks.yml:100`
- `.github/workflows/checks.yml:112`
- `.github/workflows/checks.yml:127`
- `.github/workflows/checks.yml:148`
- `.github/workflows/checks.yml:163`
- `.github/workflows/checks.yml:180`
- `.github/workflows/checks.yml:199`
- `.github/workflows/checks.yml:211`
- `.github/workflows/checks.yml:249`
- `.github/workflows/checks.yml:278`
- `.github/workflows/checks.yml:305`
- `.github/workflows/checks.yml:325`
- `.github/workflows/checks.yml:327`
- `.github/workflows/checks.yml:331`

### unpinned-uses (severity: high)

Both `uses:` references in .github/workflows/vx-tagger.yml use mutable tag-based refs instead of pinned 40-character SHA commit hashes. Failing references: `actions/checkout@v4` and `fischerscode/tagger@v0`.

Locations:

- `.github/workflows/vx-tagger.yml:15`
- `.github/workflows/vx-tagger.yml:16`

### missing-permissions (severity: medium)

.github/workflows/checks.yml has no top-level `permissions:` key and none of its jobs define job-level `permissions:` blocks. This means the workflow runs with the default (potentially broad) token permissions. All jobs (verify-dist, defaults, xcodebuild-has-exited, executable-runs, invalid-action-fails, invalid-platform-fails, invalid-swift-fails, invalid-xcode-fails, missing-api-key-id-fails, missing-api-key-issuer-id-fails, null-none-action, configurations, verbosity, swift, xcode, more-xcode, verify-codecov, verify-dot-swift-version) are affected.

Locations:

- `.github/workflows/checks.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings:
1. checks.yml: Added top-level `permissions: {}` block. Pinned all 18 occurrences of `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262` and all 3 occurrences of `mxcl/get-swift-version@v2` to SHA `fcfda7d72666658367ddd59503a19bc70c63420d`.
2. vx-tagger.yml: Pinned `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262` and `fischerscode/tagger@v0` to SHA `5ca3fa63ce3003fb7183cae547644b29f3b632be`. The file already had appropriate `permissions: contents: write` for its tagging job.

