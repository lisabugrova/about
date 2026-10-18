# Audiodoc Studio — Balance and Processing

**Artifact type:** reference test design. Original TC-BAL-001–004 IDs are preserved. Only the separately linked execution record describes historical results.

## Rules and test data

Processing time is expressed in seconds. The browser rounds a detected media duration upward to the next whole second. A known-duration file requires enough time; the batch must also fit the available balance. When duration cannot be read locally, file upload and server probing can precede the final eligibility decision.

Use MEDIA-10 (10 s), MEDIA-LONG (26:16 = 1576 s), controlled balances and separate accounts/tabs where required. A 10.01-second fixture requires 11 seconds under the current rounding rule. Verify actual fixture duration independently.

## Decision table

| Duration known? | Available balance | Demand | Expected check |
| --- | --- | --- | --- |
| Yes | 0 s | 10 s | Show 10 s shortage; normal processing start unavailable. |
| Yes | 9 s | 10 s | Show 1 s shortage; block processing. |
| Yes | 10 s | 10 s | Eligible, subject to other validation. |
| Yes | 11 s | 10 s | Eligible; eventual accounting must match the processing contract. |
| Yes | 3600 s | 1576 s | Eligible happy-path input. |
| Yes, two files | 15 s | 10 + 10 s | Each alone fits; combined batch does not. |
| Yes, fractional | 10 s | 10.01 s → 11 s | Insufficient after rounding. |
| No | Controlled balance | Determined by server probe | Do not infer free eligibility from missing metadata; verify probe, gate and feedback. |
| Changes after selection | 0 → 3600 s | 10 s | Re-evaluate the same selected file without requiring a different file. |

Insufficient and sufficient balances are disjoint equivalence classes. Equality is a boundary of the sufficient class, not a third overlapping class.

## Cases

| ID | Priority | Action / data | Expected result |
| --- | --- | --- | --- |
| TC-BAL-001 | P0 | Select MEDIA-10 with zero available time. | File may be selected; insufficient balance, 10 s shortage and no normal start action. |
| TC-BAL-002 | P0 | Select MEDIA-LONG with 3600 s; start; wait; open and download. | Ready → uploading → processing → completed; final document and downloaded content correspond to the selected media. |
| TC-BAL-003 | P1 | Repeat MEDIA-10 independently at 9/10/11 s. | 9 s blocks with 1 s shortage; 10/11 s satisfy the eligibility boundary. |
| TC-BAL-004 | P1 | Keep MEDIA-10 selected at zero balance; apply authorized test top-up to 3600 s. | Existing row refreshes eligibility and can proceed; no unauthorized live payment. |
| TC-BAL-005 | P0 | Select two 10-second files with 15 s total balance. | Combined shortage is detected; per-file readiness does not imply batch affordability. |
| TC-BAL-006 | P1 | Compare verified 10.00/10.01 s fixtures at 10/11 s balances. | Whole-second ceiling is applied consistently to eligibility, displayed shortage and the relevant transaction. |
| TC-BAL-007 | P0 | Force the authorized unknown-duration path, then return a duration above balance. | Transfer/probe may occur; transcription must not be represented as successfully affordable without the resolved check. Record handling of uploaded but unprocessed media. |
| TC-BAL-008 | P1 | Induce a controlled probe failure or invalid response. | Explicit recoverable failure; no false completed document or fabricated duration. |
| TC-BAL-009 | P0 | Complete known-duration processing; inspect document, remaining balance and history. | One intended result and a reconciled time charge. Capture starting balance, charged time, final balance and transaction identity. |
| TC-BAL-010 | P0 | In a batch, let one file finish and another fail; retry the failed file. | Successful result remains intact; retry targets failed work; no duplicate result or unexplained repeated charge. |
| TC-BAL-011 | P0 | Repeat start/retry rapidly on a controlled failure path. | No duplicate processing/charge attributable to repeated action; UI states prevent or safely handle duplication. |
| TC-BAL-012 | P0 | Use two authorized tabs with the same near-limit balance. | Resolve the server accounting contract; no unaccounted consumption, unexplained negative balance or misleading success. Local request ordering is not proof of server concurrency safety. |
| TC-BAL-013 | P1 | Interrupt network during transfer, then during processing, separately. | Stage and error are distinguishable; recovery does not lose an already completed document. |
| TC-BAL-014 | P1 | Attempt navigation/refresh while work is active; return to documents. | Warning behavior matches the supported browser; recovered list/state reflects actual completed work, not stale progress. |
| TC-BAL-015 | P1 | Expire a test session before a protected step. | No success assertion without authorized completion; reauthentication offers a clear recovery path. |
| TC-BAL-016 | P1 | Force debit/history refresh failure after a result is produced. | Inconsistent result/accounting is surfaced for investigation; a success toast alone does not establish reconciled completion. |

## State and accounting questions

Track pending → uploading → processing → completed or error, followed by a deliberate retry path. For each interruption, inspect **both** visible state and the independently available result.

Before classifying failed-processing charges, establish the approved debit/refund policy. The frontend alone cannot prove backend reservation, concurrency or compensation behavior. Record unresolved policy as a question or blocked oracle, rather than inventing a refund guarantee.

[Recorded execution](test-execution-report.md) · [Regression](regression-and-release.md) · [Coverage](product-coverage.md) · [Profile](../README.md)

