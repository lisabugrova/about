# Audiodoc Studio — Regression Test Selection & Release Summary

**Process sample and blank release-summary template.** No completed release decision is recorded here.

**Area:** File upload, available processing time, processing results  
**Regression target:** Changes to time-balance validation, selected-file queue or upload/processing flow  
**Approach:** Change impact analysis + risk-based selection.

## Impact analysis

| Affected component | Potential regression risk | Test coverage |
| --- | --- | --- |
| Available-time validation | User can start without enough time, or valid upload is blocked | TC-BAL-001, TC-BAL-002, TC-BAL-003 |
| Selected-file queue | Invalid state or stale validation after selection/removal | TC-UPL-001, TC-UPL-006 |
| Upload entry methods | Validation bypass through drag-and-drop | TC-UPL-002, TC-BAL-001 variant |
| File limits | Accepting more than 10 files or file larger than 1700 MB | TC-UPL-003, TC-UPL-004, TC-UPL-005 |
| Processing lifecycle | Stuck state, missing warning or result actions | TC-BAL-002 |
| Balance update | UI does not re-evaluate an already selected file | TC-BAL-004 |

## Prioritized regression checks

| Priority | Scenario | Selection rationale |
| --- | --- | --- |
| P0 | Balance 0 s, video 10 s: visible error and no upload action | Enforces a core business restriction |
| P0 | Balance 1 h, video 26:16: upload and result | Ensures main user journey remains available |
| P0 | Verify processing result can be downloaded and opened in editor | Core output delivery |
| P1 | 10-second video with balances **9 s / 10 s / 11 s** | Boundary and rounding risk |
| P1 | Both picker and drag-and-drop apply the same eligibility checks | Alternate entry-point risk |
| P1 | `count_01.mp4`–`count_11.mp4` (**11 × 10 s**, balance 3600 s); size **1,700,000,001 B** (decimal-MB assumption) | Enforce advertised upload constraints |
| P1 | Selected **10 s** video, balance changed **0 s → 3600 s** | Stale state risk |
| P2 | With balance 3600 s, select **10 s** MP4, clear, then select **26:16** MP4 | Recovery/usability path |
| P2 | UI behavior on supported browsers and screen sizes | Compatibility coverage |

## Exit criteria for this scope

- Priority P0 checks executed against the candidate build.
- Blocking failures reviewed; known issues and deviations tracked.
- P1/P2 exclusions and blocked checks explicitly listed.
- Changed requirements, untested environments and residual risks included in the release summary.
- Product/release owner can make a decision using the recorded evidence.

## Release test summary template

| Field | Entry |
| --- | --- |
| Release / build | |
| Environment, browser and OS | |
| Change description | |
| Test scope and selected regression IDs | |
| Total planned / executed | |
| Passed / failed / blocked / not run | |
| Open defects, severity and links | |
| Coverage gaps and exclusions | |
| Residual risk | |
| QA recommendation with rationale | |
| Release decision and owner | |

## Related artifacts

- [File Upload Test Design](audiodoc-studio-file-upload.md)
- [Balance Validation Test Design](audiodoc-studio-balance-validation.md)
- [Test Execution Summary](test-execution-report.md)

[← Portfolio](../README.md)

