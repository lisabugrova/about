# Regression checks and release assessment

Reference checklist. The release example below is fictional.

## Choose checks for the change

| Change | Essential checks | Adjacent checks |
| --- | --- | --- |
| Upload or duration | Picker/drop, cumulative count, size boundary, metadata/probe | Balance rounding, batch behavior, retry |
| Balance or payment | Below/equal/above eligibility; confirmed credit; result/charge reconciliation | History, pending state, repeated notification, multiple tabs |
| Transcript save or conversion | Edit → save → reload; original/edited content | Word timing, speaker metadata, old/current documents, exports |
| Speakers or stories | Rename/reassign/filter; story order and playback | Save/reload, source/story relationship, exports |
| Auth or consent | Sign-in/recovery, existing work, required/optional choices | Sharing roles, expiry, language, redirects |
| Sharing or projects | Owner/view/edit/denied; grouping and persistence | Direct links, revoked access, deletion/history |
| UI or localization | Changed controls, empty/loading/error states | Keyboard, narrow screen, translated units and messages |

On the release candidate, check sign-in → upload → processing → open transcript → edit → save/reload → export. Add insufficient balance and owner/nonowner checks where affected. This basic flow does not replace regression testing.

1. Record the change, build, affected users and environments. Select P0/P1 checks by risk and dependencies; list exclusions.
2. Run checks and retain evidence without private data. Separate defects from unavailable prerequisites and unclear requirements.
3. Retest fixes and adjacent behavior on the new build.
4. Recommend release or hold, including failed, blocked and untested high-risk cases. The release owner decides.

## Example: a change to saving

| Field | Assessment |
| --- | --- |
| Release | DEMO-RELEASE-1: transcript saving |
| Scope | Core journey, edit persistence, export, access, recovery |
| Planned / executed | 12 planned; 9 executed |
| Results | 8 passed; 1 failed; 1 blocked; 2 not run |
| Failed | DEMO-EDIT-01: saved correction disappears after reload |
| Blocked | Payment confirmation: sandbox provider unavailable |
| Not run | Physical iPhone journey; longer-transcript interruption |
| Remaining risk | Lost edits; incomplete payment and device coverage |
| QA recommendation | Hold: the failed save check affects the changed core workflow |

Executed means passed + failed: 9. All statuses total 12. The unavailable provider blocks testing; it is not a product defect.

To reconsider, retest the corrected save path and adjacent export/reload behavior on the new build. Complete the blocked and omitted checks or obtain the release owner's acceptance of the remaining risks. Reassess which old results still apply before carrying them forward.

The historical portfolio contains [two recorded upload scenarios](test-execution-report.md), but no completed release assessment.

[Coverage](product-coverage.md) · [Defect example](defect-report-template.md) · [Profile](../README.md)
