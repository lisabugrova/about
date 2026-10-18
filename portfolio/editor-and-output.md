# Transcript editing, audio and output

**Reference test design — not an execution report.** These cases illustrate testing of Audiodoc Studio's editor and output workflows. They are newly prepared reference material, not a record of tests executed by Elizaveta or defects she reported. No case below has a pass/fail result.

[Profile](../README.md) · [Product coverage](product-coverage.md) · [Regression and release](regression-and-release.md)

The central quality question is whether a user can correct a transcript, check it against the recording, prepare a story and obtain usable files without losing work or changing unrelated content. A successful save response or a completed download is one checkpoint; reopening the document or exported file establishes the user-facing result.

## Scope and test data

The **classic editor** supports segment editing and story assembly. The separate **beta editor** supports continuous text and rich formatting for transcripts that already contain word-level timing. Older transcripts without word-level data remain in the classic editor. Story assembly uses whole segments; it does not promise word-precise cuts or montage from several source recordings.

Use disposable documents and recordings approved for testing. Keep an unchanged baseline and identify every document and story unambiguously.

| Fixture | Contents and purpose |
|---|---|
| A — timed interview | Two speakers, an unassigned introduction, repeated words, punctuation, and a clearly audible pause between adjacent words. Retain a baseline transcript and known audio positions. |
| B — legacy transcript | Segment text and timing without word-level data; verifies compatibility with older documents. |
| C — editing sample | A timed transcript with several short paragraphs and a phrase suitable for partial-word formatting and three-word merge/split checks. |
| D — stories | Two differently named stories with distinct segment selections and orders; additional disposable stories for the ten-story limit. |
| E — long document | A representative long recording and transcript for scrolling, playback navigation and sustained editing. Record its actual size/duration instead of assuming a universal limit. |

Record the build, browser/OS, document type, access level and network condition for every run. Use owner/edit access for mutation checks and view-only access for the separate permissions regression. Include Safari and Chromium for media playback/download checks, and real mobile devices when mobile behavior is in scope.

**Priority:** P0 protects saved work or the integrity of delivered output; P1 covers core editing behavior; P2 covers secondary controls and limits. Priorities guide selection and are not defect severity ratings.

## Editing and audio integrity

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-01 | P0 | In classic editor, correct a word in fixture A, leave the field, wait for a confirmed successful save, then reload and reopen the document. | The correction persists. Unrelated text, speakers and timing match the baseline. A matching segment already present in the active story reflects the linked text correction. |
| ED-02 | P1 | Focus a classic segment and leave it without editing; repeat on fixture B. Compare saved content before and after. | No content changes. A legacy segment gains no invented word-level data. Ignore unrelated network activity when determining whether content was rewritten. |
| ED-03 | P0 | Edit several words in fixture B, save/reload, then attempt to open its beta-editor URL. | Edited text and original segment timing survive. The classic editor does not invent word timestamps. The beta entry is unavailable in normal navigation; a direct visit presents the compatibility guard and a route back. |
| ED-04 | P0 | In the beta editor, remove spaces between three measured words in fixture C. Save/reload, then restore spaces at the original word boundaries. | Original measured boundaries, including pauses, are retained through the round trip. Compare baseline and final timing in the exported JSON as well as playback. No end time precedes its start time. |
| ED-05 | P0 | Merge two words with a known pause; edit letters only inside the first word; split them again. | The untouched second word retains its measured timing. The edited span stays within its available audio bounds. Do not expect exact new internal timing where the recording was never measured at that boundary. |
| ED-06 | P0 | Insert a new word between measured words, then delete a different original word. Save/reload and inspect playback and JSON. | Unaffected measured words retain their alignment. Newly typed words are represented as ungrounded/synthetic rather than given invented measured timing. Deleted content does not remain in the saved text. |
| ED-07 | P1 | In beta editor, format half of one word and a later phrase; lengthen earlier text; save/reload. Include bold, highlight and color. | Formatting remains attached to the intended content and survives reopening; adjacent text is not accidentally styled. This checks editor persistence, not rich-format preservation in DOCX export. |
| ED-08 | P1 | In fixture A, use F4 on a sentence. Use Ctrl+Space with an unchanged caret, then after moving the caret. Switch word/sentence highlighting during playback. | F4 plays the selected sentence range. Ctrl+Space toggles playback when the caret is unchanged and starts/seeks from the new caret position when moved. Highlighting follows the audio and does not alter content. |
| ED-09 | P1 | In beta editor, press Backspace at the start of a block and Delete at the end of a block whose neighbour belongs to another speaker. Test ordinary text deletion within a block too. | Deleting across different-speaker boundaries does not silently merge their attribution. Ordinary in-block deletion remains usable. |
| ED-10 | P1 | In fixture E, alternate typing, seeking, scrolling and playback; then save/reload. In beta editor, also undo and redo a small text correction. | Edits remain accurate; playback and navigation remain usable on the recorded device. Beta undo/redo restores the intended local change. Record response times and visible stalls; classic-editor undo is currently disabled and is not an expected feature here. |

## Speakers and story versions

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-11 | P1 | Reassign the last live occurrence of speaker A to speaker B; reopen the speaker picker and reload. | The selected turn belongs to B. A remains available in the persistent speaker roster, allowing reassignment back. Other speakers and their turns are unchanged. |
| ED-12 | P0 | Globally rename one speaker with multiple source occurrences and matching segments in the active story. Save/reload both. | Intended source and active-story occurrences use the new name; unrelated labels remain unchanged. Record inactive-story behavior separately—global propagation to every stored version is not assumed. |
| ED-13 | P1 | Cycle speaker display modes, filter one speaker and highlight another; clear the controls and reload. Include an unassigned introduction. | Show-all/change-only/none changes label visibility appropriately. Filtering hides the selected speaker's turns; highlighting dims other turns. Display controls do not remove transcript content. Record assumption-badge behavior separately from editorial speaker names. |
| ED-14 | P0 | Add noncontiguous source segments to a story, change their order, remove one segment, rename the story and reload. | The chosen content, order and name persist; removed story segments remain in the source. Story playback follows the assembled order. Confirm text edits separately because source and active-story text can be linked. |
| ED-15 | P0 | Edit story A, immediately switch A→B→A, then reload both stories. Repeat with delayed save responses and clearly different content in A/B. | Each story retains its own latest successfully saved content and name. A's edits do not overwrite B. No successful-save claim is made until server persistence is checked. |
| ED-16 | P0 | Start loading uncached story A on a slow connection, then select already cached story B before A completes. | B remains selected with B's content after the late A response arrives. The dropdown, visible text, playback target and subsequent save target agree. |
| ED-17 | P0 | Delete the active story, accept confirmation, switch among remaining stories and reload. Then create a new story. | The deleted story stays deleted and is not recreated by an outgoing save. Remaining stories are intact. A newly created story starts empty and uses an available slot. |
| ED-18 | P2 | Reach ten stories, attempt to add another, delete one and create a replacement. Also cancel a deletion. | No eleventh story is created through the UI. Deleting one frees a slot. Cancelling deletion preserves the story and its content. |

## Save, recovery and restoration

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-19 | P0 | Make distinct source and story edits, confirm their save responses, close/reopen and compare with the baseline and intended final state. | Both documents contain exactly the committed edits in their correct targets. A successful response alone is not the persistence oracle. |
| ED-20 | P0 | On disposable data, edit then immediately reload/navigate away, hide the tab or switch mobile apps. Repeat after a confirmed save. Then repeat with a failed save followed by network recovery. | Confirmed saves survive reopening. For interrupted or failed saves, record what remains locally, what reaches the server and what the user is told. Offline recovery and guaranteed saving during app termination are not assumed; any loss is reported with exact timing and conditions. |
| ED-21 | P0 | Edit a disposable source document. Choose Restore Original and cancel; then repeat and confirm. Reload and inspect source and stories. | Cancel changes nothing. Confirm restores the source backup and persists it. Document the effect on active and inactive stories against the agreed restoration contract rather than assuming they reset too. |

## Output verification

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-22 | P0 | Export source and a differently ordered story to TXT, DOCX and JSON; copy each to the clipboard. Open the actual files and paste copied text into a plain-text editor. | Each output contains the correct target and segment order, including recent committed corrections. TXT/DOCX group speaker turns as implemented; JSON preserves available timing/metadata. DOCX opens correctly; editor styling is not assumed to carry into DOCX. |
| ED-23 | P0 | Make a text edit and leave edit mode so it reaches the editor's current state; export SRT before autosave completes. Repeat with the subtitle service unavailable. | Default export reflects the current edited content and produces readable cues with valid timestamps. When the service fails, verify the legacy fallback separately; its segment-based cue layout may differ. Check actual playback alignment and record the path used. |
| ED-24 | P1 | Double-click SRT and REAPER export while each is in progress; repeat after completion and with an export failure. | Repeated clicks on the same in-flight action do not start unintended duplicate work. Loading controls resolve, and the action can be retried. Verify the file, not just the loading indicator. |
| ED-25 | P0 | Export source and reordered story as REAPER ZIPs. Extract them and open each project in REAPER with its bundled audio. | Both project and audio files are usable; references resolve; the intended content, segment timing and story order are represented. Check the media contents and playback, including the Safari download path, rather than trusting the archive extension. |

## Contract questions to resolve before execution

- **Subtitle rules:** agree the intended reading-speed, line-length, language and timing rules for backend-generated cues. The web export path alone cannot establish the complete transcription/subtitle-service contract. Treat fallback output as a separate mode.
- **Stories and shared edits:** specify whether speaker/text changes should affect inactive story versions and whether restoring the source should change any story. Current active-story coupling must not be confused with a promise of global synchronisation.
- **Save failures:** agree what the user must see, how retries should work and what constitutes acceptable recovery after navigation, disconnection or app termination. Do not award a pass based on an unspecified recovery promise.
- **Speaker suggestions:** distinguish AI-generated assumptions from user-assigned labels. Confirm when badges should persist, change or disappear; behavior is not uniform across every current edit path.
- **Timing and usability tolerances:** define acceptable alignment error and response-time targets for the chosen devices and document sizes. Exact recovery of existing measured boundaries is different from estimating a new split inside an unmeasured span.

For execution, attach the fixture identity, relevant before/after evidence, observed result and any defect link to each selected case. Keep unexplored cases marked **Not run** and unresolved contracts **Blocked / clarification required** in the [release record](regression-and-release.md).
