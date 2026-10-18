# Compatibility and Exploratory Testing

**Artifact type:** reference test design, not a completed browser matrix.

## Environment selection

Use the release's supported-browser policy. A useful initial sample is Chrome on Windows/macOS, Safari on macOS/iPhone and Chrome on Android, with exact versions recorded. These are proposed environments, not a claim that every browser or device is supported or tested. A narrow browser viewport is not a physical-device result.

| ID | Priority | Check | Expected observation |
| --- | --- | --- | --- |
| CX-01 | P1 | Run one core upload → process → open → export journey in each agreed environment. | Record actual capability and output; no assumption that desktop success proves mobile success. |
| CX-02 | P1 | Compare file picker, desktop drag/drop and mobile file selection. | Available entry paths work; unsupported interaction is not the only route. |
| CX-03 | P1 | Exercise audio playback, seek and export with representative codecs. | User can verify text against sound; unsupported media yields understandable feedback. |
| CX-04 | P1 | At narrow widths, zoom and large text, open errors, menus and editor actions. | Essential text/actions remain readable and reachable without obstructive overlap. |
| CX-05 | P1 | Use keyboard-only navigation through sign-in, upload, dialogs and editor controls. | Logical focus, visible focus, operable controls and escape/close behavior; text editing shortcuts do not unintentionally activate navigation. |
| CX-06 | P1 | Inspect labels, required fields, validation feedback and dialogs with an accessibility tool or screen reader. | Controls and errors are identifiable; dynamic status is understandable. Record actual observations; this is not a compliance certification. |
| CX-07 | P1 | Switch English/Russian around sign-in, error states, balance and exports. | Correct active language, understandable units, no raw translation keys, stable user data. |
| CX-08 | P1 | Paste Cyrillic, Latin, emoji, punctuation and multiline text into editable content. | Save/reload/export preserve supported characters and intended structure. |
| CX-09 | P1 | Refresh, background the mobile browser and restore a tab at defined workflow stages. | Saved state remains consistent; interrupted work has truthful status and recovery. |
| CX-10 | P1 | Use an approved longer transcript and larger supported media with constrained network. | Observe responsiveness, progress and recoverability; report measured conditions rather than inventing a performance SLA. |
| CX-11 | P1 | Deny optional browser capability/permission or trigger clipboard failure. | Failure feedback is clear and a manual alternative is available where specified. |
| CX-12 | P1 | Leave an editor open, change permissions from the owner account, then try save/reload. | Current access contract is enforced; stale UI does not establish continuing authorization. |

## Time-boxed exploratory charters

| Charter | Mission | Evidence to return |
| --- | --- | --- |
| First useful result — 30 min | Start from a new authorized account and reach a usable transcript without undocumented guidance. | Path, friction, unclear states, blockers, exact environment |
| Interrupt and recover — 30 min | Challenge transfer, processing and saving independently with controlled network interruptions. | Stage-specific state, retained work, recovery steps, accounting questions |
| Two accounts, one document — 30 min | Compare private/view/edit and revoked access using authorized accounts. | Read/write/delete/share matrix and reproducible deviations |
| Edit without losing meaning — 45 min | Correct repeated words, punctuation and speakers; reorder story material; save/reload/export. | Before/after text, audio correspondence and exported content |
| Follow the time — 30 min | Follow sandbox credit through known-duration processing and history. | Starting/ending balances, result identity, charge identity, unresolved rules |

Each charter ends with findings, tested boundaries, unanswered questions and the next highest-value check. No defect count is required; a clean session still records what was investigated.

[Coverage](product-coverage.md) · [Regression](regression-and-release.md) · [Profile](../README.md)

