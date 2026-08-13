<!-- markdownlint-disable -->

# Hardening Report: AlexSim93--pull-request-analytics-action/v4.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **AlexSim93--pull-request-analytics-action/v4.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file .github/workflows/unit-tests.yml has no top-level `permissions:` key and the single job `unit-tests` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default (potentially broad) repository permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/unit-tests.yml:1`

### unpinned-uses (severity: high)

The workflow file .github/workflows/unit-tests.yml references two actions using mutable version tags instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Failing references:
- `uses: actions/checkout@v3` (line 7)
- `uses: actions/setup-node@v3` (line 8)

These should be pinned to their full SHA digests, e.g.:
- `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`
- `uses: actions/setup-node@1a4442cacd436585916779262731d1f29db0f5a8 # v3`

Locations:

- `.github/workflows/unit-tests.yml:7`
- `.github/workflows/unit-tests.yml:8`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed .github/workflows/unit-tests.yml: (1) Added `permissions: {}` at the top level to enforce least privilege — the workflow only runs npm install and tests and requires no GitHub API access. (2) Pinned `actions/checkout@v3` to SHA `f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3` and `actions/setup-node@v3` to SHA `3235b876344d2a9aa001b8d1453c930bba69e610 # v3` to prevent supply-chain attacks via mutable tags.

