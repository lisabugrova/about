# Compatibility and Exploratory Testing

**QA reference:** proposed checks and exploratory sessions. [Scope and provenance](README.md).

## Environment selection

Choose environments from the release's supported-browser list and record exact versions. A starting sample is Chrome on Windows/macOS, Safari on macOS/iPhone and Chrome on Android. Run mobile checks on physical devices as well as narrow desktop windows. The accessibility checks below are a sample, not a full conformance audit.

| ID | Priority | Check | Expected observation |
| --- | --- | --- | --- |
| CX-01 | P1 | Run upload → process → open → export in each agreed environment. | The journey completes and produces a usable file in each environment; record results separately. |
| CX-02 | P1 | Compare file picker, desktop drag/drop and mobile file selection. | Each supported path works; users have a file-selection option on every supported device. |
| CX-03 | P1 | Exercise audio playback, seek and export with representative codecs. | User can verify text against sound; unsupported media yields understandable feedback. |
| CX-04 | P1 | At narrow widths, zoom and large text, open errors, menus and editor actions. | Essential text/actions remain readable and reachable without obstructive overlap. |
| CX-05 | P1 | Use only the keyboard through sign-in, upload, dialogs and editor controls. | Focus is visible and follows a logical order; controls and dialog closing work. Editing shortcuts do not trigger unwanted navigation. |
| CX-06 | P1 | Inspect labels, required fields, validation feedback and dialogs with an accessibility tool or screen reader. | Controls, errors and changing status are identifiable and understandable. |
| CX-07 | P1 | Switch English/Russian around sign-in, error states, balance and exports. | The selected language and clear units appear throughout; no raw translation keys appear and user data is unchanged. |
| CX-08 | P1 | Paste Cyrillic, Latin, emoji, punctuation and multiline text into editable content. | Save/reload/export preserve supported characters and intended structure. |
| CX-09 | P1 | Refresh, background the mobile browser and restore a tab at defined workflow stages. | Saved work persists; status reflects completed and interrupted work, with recovery where supported. |
| CX-10 | P1 | Use an approved longer transcript and larger supported media on a constrained network. | Record response times, progress and recovery under the measured conditions; compare with agreed targets, if any. |
| CX-11 | P1 | Deny optional browser capability/permission or trigger clipboard failure. | Failure feedback is clear and a manual alternative is available where specified. |
| CX-12 | P1 | Leave an editor open, change permissions from the owner account, then try save/reload. | Save and reload enforce the updated permissions, even if the old editor remains visible. |

## Exploratory sessions

| Session | Task | Record |
| --- | --- | --- |
| First useful result — 30 min | Start from a new test account and reach a usable transcript using the available guidance. | Steps, difficulties, unclear states, blockers, exact environment |
| Interrupt and recover — 30 min | Challenge transfer, processing and saving independently with controlled network interruptions. | Stage-specific state, retained work, recovery steps, accounting questions |
| Two accounts, one document — 30 min | Compare private/view/edit and revoked access using authorized accounts. | Read/write/delete/share matrix and reproducible deviations |
| Edit without losing meaning — 45 min | Correct repeated words, punctuation and speakers; reorder story material; save/reload/export. | Before/after text, audio correspondence and exported content |
| Follow the time — 30 min | Follow sandbox credit through known-duration processing and history. | Starting/ending balances, result identity, charge identity, unresolved rules |

Record findings, tested boundaries, open questions and the next useful check after each session, including sessions with no defects found.

[Coverage](product-coverage.md) · [Regression](../portfolio/regression-and-release.md) · [Profile](../README.md)

