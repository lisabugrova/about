# Audiodoc Studio — Product Coverage and Risk Map

**Artifact type:** reference test design, not an execution report.

This map covers the main user-facing domains in the reviewed application. It defines what to investigate and where to find the detailed cases; it does not assert that every feature was personally tested or that every listed behavior has passed.

## Risk priorities

- **P0:** protects access to work, saved content, correct account permissions and processing-time accounting; failures can block the core journey or cause material harm.
- **P1:** important feature correctness, recovery and compatibility.
- **P2:** lower-impact presentation and convenience, escalated when they prevent real use.

Priority depends on the release change and affected users. An inaccessible critical control is not merely cosmetic.

## Domain coverage

| Domain | User value | Main failure risk | Reference |
| --- | --- | --- | --- |
| Registration, login and recovery | Reach the right account and existing work | Lockout, lost continuity, wrong identity | [Accounts](accounts-payments-and-access.md) |
| Required and optional consent | Make informed choices without losing the flow | Incorrect gating, unwanted optional consent | [Accounts](accounts-payments-and-access.md) |
| File selection and metadata | Submit intended media | Rejected valid file, invalid queue, wrong duration | [Upload](audiodoc-studio-file-upload.md) |
| Balance and processing | Receive the result for the correct time charge | Incorrect eligibility, repeated charge, misleading success | [Balance](audiodoc-studio-balance-validation.md) |
| Transcript and audio | Verify and correct the text | Misaligned playback, lost or corrupted text | [Editor](editor-and-output.md) |
| Speaker tools | Attribute speech consistently | Incorrect assignment, hidden content, renamed wrong speaker | [Editor](editor-and-output.md) |
| Stories | Assemble selected material | Wrong order, stale content, source/story confusion | [Editor](editor-and-output.md) |
| Saving and recovery | Retain work across navigation | Silent data loss, stale writes, misleading save state | [Editor](editor-and-output.md) |
| Exports | Use the result outside the application | Missing text, broken timing, wrong speaker/content | [Editor](editor-and-output.md) |
| Projects and document lifecycle | Organize, find and remove work | Orphaned documents, unintended removal | [Accounts](accounts-payments-and-access.md) |
| Sharing and permissions | Grant appropriate access | Unauthorized reading/writing/deletion | [Accounts](accounts-payments-and-access.md) |
| Purchases, history and referrals | Understand credited and spent time | Wrong charge/credit, duplicate rewards | [Accounts](accounts-payments-and-access.md) |
| Language, browser and interaction | Complete the journey in the user's environment | Unusable controls, untranslated errors, inconsistent behavior | [Compatibility](compatibility-and-exploration.md) |

## Contracts that change the expected result

- The queue permits ten files in total, including incremental selections.
- The displayed “1700 MB” size limit is implemented as **1,782,579,200 bytes (1700 MiB)**.
- Detected media duration is rounded upward to whole seconds. Unknown duration has a server-probe path; file transfer and transcription eligibility are separate stages.
- A shared document still requires authentication. Private, view and edit permissions are distinct.
- The current upload flow uses the current model with language, diarization and verbatim options; a visible model selector is not assumed.
- The newer word-based editor is a separate beta path. Old-document handling and supported features must be checked separately.
- Self-service payment refunds are disabled in the reviewed snapshot; do not invent a successful refund journey.

## Test-data pack

| Fixture | Contents / purpose |
| --- | --- |
| MEDIA-10 | Valid 10-second MP4 for balance and small-file checks |
| MEDIA-LONG | Valid 26:16 media for the recorded sufficient-balance scenario |
| COUNT-01…11 | Distinct valid 10-second files, with enough balance to isolate count limits |
| SIZE-LOW/EQUAL/HIGH | Valid media at 1,782,579,199 / 1,782,579,200 / 1,782,579,201 bytes |
| DURATION | Valid fixtures around a whole-second boundary and one verified server-probe path |
| TRANSCRIPT | Synthetic two-speaker text with known audio, punctuation, repeated words and segment order |
| LEGACY | Authorized older document without word-level data plus a current word-level document |
| ACCOUNTS | Owner A, unrelated signed-in B, and signed-out browser; separate controlled balances |
| PAYMENT | Sandbox payments for pending, confirmed, rejected and repeated notification states |

## Release threads

1. **Core:** sign in → select media → verify eligibility → process → open → edit → save/reload → export.
2. **Access:** owner creates → grants view/edit → second account opens → permissions change → access rechecked.
3. **Accounting:** acquire test balance → process known duration → reconcile result, remaining time and history.
4. **Recovery:** interrupt a defined stage → inspect retained state → retry safely → verify final content and accounting.

Separate service availability and provider sandbox access are prerequisites, not candidate performance measures.

[Design index](test-design-workbook.md) · [Regression and release](regression-and-release.md) · [Profile](../README.md)

