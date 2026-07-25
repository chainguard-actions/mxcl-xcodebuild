<!-- markdownlint-disable -->

# Hardening Report: mxcl--xcodebuild/v3.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **mxcl--xcodebuild/v3.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag or version refs instead of pinned 40-character SHA commits, making the workflows vulnerable to supply-chain attacks if the referenced tags are moved or compromised.

In `.github/workflows/checks.yml`:
- `actions/checkout@v5` (multiple jobs: verify-dist, defaults, xcodebuild-has-exited, executable-runs, invalid-action-fails, invalid-platform-fails, invalid-swift-fails, invalid-xcode-fails, missing-api-key-id-fails, missing-api-key-issuer-id-fails, null-none-action, configurations, verbosity, swift, xcode, verify-codecov, verify-dot-swift-version)
- `actions/checkout@v4` (verify-sanitizers job)
- `mxcl/get-swift-version@v2` (swift job, verify-dot-swift-version job)

In `.github/workflows/vx-tagger.yml`:
- `actions/checkout@v5`
- `fischerscode/tagger@v0`

Locations:

- `.github/workflows/checks.yml:18`
- `.github/workflows/checks.yml:25`
- `.github/workflows/checks.yml:35`
- `.github/workflows/checks.yml:47`
- `.github/workflows/checks.yml:57`
- `.github/workflows/checks.yml:70`
- `.github/workflows/checks.yml:83`
- `.github/workflows/checks.yml:96`
- `.github/workflows/checks.yml:109`
- `.github/workflows/checks.yml:124`
- `.github/workflows/checks.yml:145`
- `.github/workflows/checks.yml:162`
- `.github/workflows/checks.yml:179`
- `.github/workflows/checks.yml:200`
- `.github/workflows/checks.yml:204`
- `.github/workflows/checks.yml:232`
- `.github/workflows/checks.yml:261`
- `.github/workflows/checks.yml:278`
- `.github/workflows/checks.yml:282`
- `.github/workflows/vx-tagger.yml:13`
- `.github/workflows/vx-tagger.yml:14`

### missing-permissions (severity: medium)

`.github/workflows/checks.yml` has no top-level `permissions:` key and no job-level `permissions:` block on any of its jobs. Without explicit permissions, the workflow inherits the default repository permissions (which may include `contents: write` and other broad scopes depending on repository settings), violating the principle of least privilege.

Locations:

- `.github/workflows/checks.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned `uses:` references in both workflow files by replacing mutable tags with pinned 40-character SHA commits (preserving tags as comments): actions/checkout@v5 → fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 (17 occurrences in checks.yml, 1 in vx-tagger.yml), actions/checkout@v4 → 11d5960a326750d5838078e36cf38b85af677262 (1 occurrence in checks.yml), mxcl/get-swift-version@v2 → fcfda7d72666658367ddd59503a19bc70c63420d (3 occurrences in checks.yml), fischerscode/tagger@v0 → 5ca3fa63ce3003fb7183cae547644b29f3b632be (1 occurrence in vx-tagger.yml). Added `permissions: {}` top-level block to checks.yml to enforce least privilege. The vx-tagger.yml already had an appropriate `permissions: contents: write` block required for the tagger action.

