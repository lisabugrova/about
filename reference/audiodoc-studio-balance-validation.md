# Audiodoc Studio — Balance and Processing

**Reference test design.** Cases TC-BAL-001–004 adapt the original samples; the remaining cases extend the plan. Identifiers are retained, but wording and expected results were revised during source review. [Authorship and sources](README.md).

## Rules and test data

Time is measured in seconds. The browser rounds media duration up to a whole second. Available time must cover each file and the complete batch. If local metadata fails, upload and a server probe may precede the final balance check.

Use MEDIA-10 (10 s), MEDIA-LONG (26:16 = 1576 s), controlled balances and separate accounts/tabs where needed. Verify file duration independently: 10.01 seconds requires 11 seconds. Use the approved payment sandbox for top-ups.

## Decision table

| Duration known? | Available balance | Demand | Expected result |
| --- | --- | --- | --- |
| Yes | 0 s | 10 s | Show 10 s shortage; processing cannot start. |
| Yes | 9 s | 10 s | Show 1 s shortage; block processing. |
| Yes | 10 s | 10 s | Enough time; other checks still apply. |
| Yes | 11 s | 10 s | Enough time; verify the final charge. |
| Yes | 3600 s | 1576 s | Enough time for the successful path. |
| Yes, two files | 15 s | 10 + 10 s | Each alone fits; combined batch does not. |
| Yes, fractional | 10 s | 10.01 s → 11 s | Insufficient after rounding. |
| No | Controlled balance | Determined by server probe | Probe, check balance and show the result; missing metadata does not mean free processing. |
| Changes after selection | 0 → 3600 s | 10 s | Update the selected file's readiness without replacing it. |

The two equivalence classes are insufficient and sufficient balance. Equality belongs to the sufficient class.

## Cases

| ID | Priority | Action / data | Expected result |
| --- | --- | --- | --- |
| TC-BAL-001 | P0 | Select MEDIA-10 with zero available time. | File can be selected; 10 s shortage shown; processing cannot start. |
| TC-BAL-002 | P0 | Select MEDIA-LONG with 3600 s; start; wait; open and download. | Ready → uploading → processing → completed; document and download match the selected media. |
| TC-BAL-003 | P1 | Test MEDIA-10 separately at 9/10/11 s. | 9 s blocks with 1 s shortage; 10/11 s pass the balance check. |
| TC-BAL-004 | P1 | Keep MEDIA-10 selected at zero balance; top up to 3600 s in the sandbox. | Existing row updates and processing can start. |
| TC-BAL-005 | P0 | Select two 10-second files with 15 s total balance. | Batch shortage detected even though each file fits separately. |
| TC-BAL-006 | P1 | Compare verified 10.00/10.01 s files at 10/11 s balances. | Rounding up is consistent across eligibility, displayed shortage and the transaction. |
| TC-BAL-007 | P0 | Simulate unknown duration, then return a duration above balance. | Upload/probe may finish; balance must be checked before allowing transcription. Record what happens to uploaded, unprocessed media. |
| TC-BAL-008 | P1 | Simulate probe failure or an invalid response. | Recoverable error; no false completion or invented duration. |
| TC-BAL-009 | P0 | Finish known-duration processing; inspect document, balance and history. | One result; charge reconciles. Record starting balance, charged time, final balance and transaction ID. |
| TC-BAL-010 | P0 | Let one batch file finish and another fail; retry the failed file. | Completed result remains; only failed work retries; no duplicate result or unexplained repeat charge. |
| TC-BAL-011 | P0 | Rapidly repeat start/retry on a controlled failure path. | Repeated actions are blocked or handled without duplicate processing or charges. |
| TC-BAL-012 | P0 | Use two tabs with the same near-limit account balance. | Check server accounting: no unaccounted use, unexplained negative balance or false success. Local request order does not prove server concurrency safety. |
| TC-BAL-013 | P1 | Interrupt the network during transfer and processing in separate tests. | Error identifies the stage; recovery preserves completed documents. |
| TC-BAL-014 | P1 | Navigate away or refresh during active work; return to documents. | Warnings work as supported by the browser; restored status matches actual work. |
| TC-BAL-015 | P1 | Expire a test session before a protected step. | No false success; signing in again provides a clear recovery path. |
| TC-BAL-016 | P1 | Simulate debit/history refresh failure after a result is produced. | Result/accounting mismatch is visible; a success notification alone does not prove correct accounting. |

## States and accounting

Track pending → uploading → processing → completed or error, then retry. After interruptions, compare the UI state with the actual result.

Confirm debit/refund rules before reporting charges for failed processing as defects. The frontend cannot prove server reservation, concurrency or compensation behavior. If a rule is unclear, record the open question and mark the expected result undetermined.

[Recorded execution](../portfolio/test-execution-report.md) · [Regression](../portfolio/regression-and-release.md) · [Coverage](product-coverage.md) · [Profile](../README.md)

