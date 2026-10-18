# Regression Selection and Release Assessment

**Artifact type:** process reference with a clearly labeled illustrative release example.

## Select by change impact

| Change | Essential checks | Adjacent checks |
| --- | --- | --- |
| Upload or duration handling | Picker/drop, cumulative count, size boundary, metadata/probe | Balance rounding, batch behavior, retry |
| Balance or payment | Below/equal/above eligibility; confirmed credit; result/charge reconciliation | History, pending state, repeated notification, multiple tabs |
| Transcript save or conversion | Edit → save → reload; original/edited content | Word timing, speaker metadata, old/current documents, exports |
| Speaker/story behavior | Rename/reassign/filter; story order and playback | Save/reload, source/story relationship, exports |
| Auth/consent | Sign-in/recovery, existing work, required/optional choices | Sharing roles, expiry, language and redirects |
| Sharing/project change | Owner/view/edit/denied; grouping and persistence | Direct links, revoked access, deletion/history |
| UI/localization | Changed controls, empty/loading/error states | Keyboard, narrow screen, translated units and feedback |

## Minimum release thread

On the actual candidate build, select cases covering sign-in → valid upload → processing → open transcript → edit → save/reload → export. Add insufficient balance and owner/nonowner checks where the change can affect them.

A green smoke thread is not a full regression pass. Include failed, blocked and untested high-risk checks in the recommendation.

## Execution and decision rules

1. Identify the change, target build, affected users and environments.
2. Select P0/P1 checks by risk and dependencies; document exclusions.
3. Run and record actual results with sanitized evidence.
4. Triage failures; distinguish defects, blocked prerequisites and unclear requirements.
5. Retest fixes on the new build and adjacent flows.
6. Give a reasoned QA recommendation; the accountable release owner makes the release decision.

## Worked example: a save-related release

**Illustrative only — fictional build, counts and findings. This is not an Audiodoc release history.**

| Field | Example assessment |
| --- | --- |
| Release | DEMO-RELEASE-1, a hypothetical change to transcript saving |
| Scope | Core journey, edit persistence, export, access and recovery |
| Planned / executed | 12 planned; 9 executed |
| Results | 8 passed; 1 failed; 1 blocked; 2 not run |
| Failed | DEMO-EDIT-01: saved correction disappears after reload |
| Blocked | Payment confirmation: sandbox provider unavailable |
| Not run | Physical iPhone journey; longer-transcript interruption |
| Residual risk | User edits may be lost; payment and device coverage incomplete |
| QA recommendation | Hold this release because the failed persistence check affects the changed core workflow |
| Decision | Illustrative recommendation only; no real release was approved or blocked |

Counts reconcile: 8 + 1 + 1 + 2 = 12; executed is passed + failed = 9. The blocked provider is an environment condition, not a fictional product defect.

### What changes the recommendation?

Retest the corrected persistence path and adjacent export/reload behavior on the new build. Complete the blocked and omitted checks or have the release owner explicitly accept their documented residual risk. Do not copy the old pass status to a new build without deciding which results remain relevant.

## Existing historical evidence

The supplied portfolio contains [two recorded upload scenarios](test-execution-report.md). It does not contain a completed historical release decision. The example above demonstrates the reporting structure without manufacturing one.

[Coverage](product-coverage.md) · [Defect example](defect-report-template.md) · [Profile](../README.md)

