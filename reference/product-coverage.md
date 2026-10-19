# Audiodoc Studio — Product Coverage and Risk Map

**QA reference:** coverage and test data for the main user workflows. [Scope and provenance](README.md).

## Risk priorities

- **P0:** account access, saved work, permissions and time accounting; failures can block the core workflow or cause data or financial loss.
- **P1:** other important functions, recovery and compatibility.
- **P2:** presentation and convenience with limited user impact.

Adjust priorities to the release and affected users. A layout problem that blocks a critical action takes that action's priority.

## Domain coverage

| Domain | User value | Main failure risk | Reference |
| --- | --- | --- | --- |
| Registration, login and recovery | Reach the right account and existing work | Lockout, lost account history, wrong identity | [Accounts](accounts-payments-and-access.md) |
| Required and optional consent | Make and retain consent choices | Incorrect signup blocking, unwanted optional consent | [Accounts](accounts-payments-and-access.md) |
| File selection and metadata | Submit intended media | Rejected valid file, invalid queue, wrong duration | [Upload](audiodoc-studio-file-upload.md) |
| Balance and processing | Receive the result for the correct time charge | Incorrect eligibility, repeated charge, misleading success | [Balance](audiodoc-studio-balance-validation.md) |
| Transcript and audio | Verify and correct the text | Misaligned playback, lost or corrupted text | [Editor](editor-and-output.md) |
| Speaker tools | Attribute speech correctly | Wrong speaker, hidden content, unintended renaming | [Editor](editor-and-output.md) |
| Stories | Assemble selected material | Wrong order, stale content, source/story confusion | [Editor](editor-and-output.md) |
| Saving and recovery | Retain work across navigation | Silent data loss, stale writes, misleading save state | [Editor](editor-and-output.md) |
| Exports | Use the result outside the application | Missing text, broken timing, wrong speaker/content | [Editor](editor-and-output.md) |
| Projects and document lifecycle | Organize, find and remove work | Orphaned documents, unintended removal | [Accounts](accounts-payments-and-access.md) |
| Sharing and permissions | Grant appropriate access | Unauthorized reading/writing/deletion | [Accounts](accounts-payments-and-access.md) |
| Purchases, history and referrals | Understand credited and spent time | Wrong charge/credit, duplicate rewards | [Accounts](accounts-payments-and-access.md) |
| Language, browser and interaction | Complete the journey in the user's environment | Unusable controls, untranslated errors, inconsistent behavior | [Compatibility](compatibility-and-exploration.md) |

## Product rules to check

- The queue permits ten files in total, including incremental selections.
- The displayed “1700 MB” size limit is implemented as **1,782,579,200 bytes (1700 MiB)**.
- Detected media duration is rounded up to whole seconds. The server can probe files with unknown duration, so transfer may precede the final transcription eligibility check.
- Shared documents require sign-in. Test private, view and edit permissions separately.
- Upload settings include language, diarization and verbatim transcription. The reviewed flow uses the current model without a visible model selector.
- The word-based editor is a separate beta. Check its supported features and handling of older documents separately.
- Self-service payment refunds are disabled in the reviewed version.

## Test-data pack

| Fixture | Contents / purpose |
| --- | --- |
| MEDIA-10 | Valid 10-second MP4 for balance and small-file checks |
| MEDIA-LONG | Valid 26:16 media for the recorded sufficient-balance scenario |
| COUNT-01…11 | Distinct valid 10-second files, with enough balance to isolate count limits |
| SIZE-LOW/EQUAL/HIGH | Valid media at 1,782,579,199 / 1,782,579,200 / 1,782,579,201 bytes |
| DURATION | Valid files around a whole-second boundary and a file confirmed to require server probing |
| TRANSCRIPT | Synthetic two-speaker text with known audio, punctuation, repeated words and segment order |
| LEGACY | Authorized older document without word-level data plus a current word-level document |
| ACCOUNTS | Owner A, unrelated signed-in B, and signed-out browser; separate controlled balances |
| PAYMENT | Sandbox payments for pending, confirmed, rejected and repeated notification states |

## End-to-end release checks

1. **Core:** sign in → select media → verify eligibility → process → open → edit → save/reload → export.
2. **Access:** owner creates → grants view/edit → second account opens → permissions change → access rechecked.
3. **Accounting:** acquire test balance → process known duration → reconcile result, remaining time and history.
4. **Recovery:** interrupt a defined stage → inspect retained state → retry → verify final content and accounting.

Confirm service availability and payment-sandbox access before running the relevant checks.

[Design index](README.md) · [Regression and release](../portfolio/regression-and-release.md) · [Profile](../README.md)

