# Transcript editing, audio and output

**Reference test design.** For the origin and status of these cases, see the [test design workbook](test-design-workbook.md).

[Profile](../README.md) · [Product coverage](product-coverage.md) · [Regression and release](regression-and-release.md)

Check that users can correct a transcript, listen back, assemble a story and export usable files without losing work. Verify results by reopening documents and files.

## Scope and test data

The **classic editor** supports segment editing and stories. The **beta editor** adds continuous text and rich formatting for transcripts with word-level timing. Older transcripts stay in classic. Stories use whole segments from one recording, without word-precise cuts.

Use approved test recordings, named documents and stories, and an unchanged baseline.

| Fixture | Contents and purpose |
|---|---|
| A — timed interview | Two speakers, unassigned introduction, repeated words, punctuation and an audible pause between words. Note control audio positions. |
| B — legacy transcript | Segment text and timing, without word-level data. |
| C — editing sample | Timed short paragraphs for partial-word formatting and three-word merge/split checks. |
| D — stories | Two named stories with different selections and orders; extra stories for the ten-story limit. |
| E — long document | Representative long recording for sustained editing, seeking and scrolling. Record its size and duration. |

Record build, browser/OS, document type, access and network conditions. Edit as owner/editor; test view-only permissions separately. Include Safari and Chromium for playback/downloads, and real devices for mobile checks.

**Priority:** P0 — saved work and output integrity; P1 — core editing; P2 — secondary controls and limits. Test priority is separate from defect severity.

## Editing and audio integrity

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-01 | P0 | Classic, A: correct a word, leave the field, confirm save, then reopen. | Correction persists in source and the matching active-story segment. Other text, speakers and timing match the baseline. |
| ED-02 | P1 | Focus and leave a classic segment without editing; repeat on B. Compare saved content. | No content changes or invented word data. Distinguish content writes from unrelated network requests. |
| ED-03 | P0 | Edit B, save/reload, then open its beta-editor URL. | Text and segment timing survive without invented word timestamps. Normal navigation hides beta; a direct visit shows the compatibility guard and link back. |
| ED-04 | P0 | Beta, C: merge three measured words by deleting spaces. Save/reload, then restore spaces at the original boundaries. | JSON and playback retain the original boundaries and pauses. No end time precedes its start. |
| ED-05 | P0 | Merge two words with a known pause; change letters in the first; split again. | Second word retains exact timing; edited span stays within available bounds. A new split without an original measurement can only be estimated. |
| ED-06 | P0 | Insert a word between measured words; delete a different original word. Save/reload; inspect playback and JSON. | Untouched words retain alignment. Insertions are synthetic/ungrounded; deleted text is absent. |
| ED-07 | P1 | Beta: format half a word and a later phrase using bold, highlight and color. Lengthen earlier text; save/reload. | Styling stays on the intended content, survives reopening and leaves neighbours unchanged. DOCX formatting is checked separately. |
| ED-08 | P1 | A: press F4; use Ctrl+Space before and after moving the caret. Switch word/sentence highlighting during playback. | F4 plays the sentence. Ctrl+Space toggles playback with an unchanged caret and starts/seeks from a moved caret. Highlight follows audio without changing content. |
| ED-09 | P1 | Beta: Backspace/Delete at boundaries between different-speaker blocks, then within a block. | Different-speaker blocks are not silently joined; ordinary deletion works. |
| ED-10 | P1 | E: alternate typing, seeking, scrolling and playback; save/reload. In beta, undo/redo a correction. | Changes persist; playback/navigation stay usable. Beta undo/redo restores the change; classic undo is disabled. Record response times and stalls. |

## Speakers and story versions

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-11 | P1 | Reassign speaker A's last turn to B; reopen the picker and reload. | Turn belongs to B; A remains in the roster for reassignment back. Other turns are unchanged. |
| ED-12 | P0 | Globally rename a speaker appearing repeatedly in source and active story. Save/reload both. | All intended occurrences use the new name; other labels stay unchanged. Check inactive versions separately; propagation to all stories is not established. |
| ED-13 | P1 | With an unassigned intro, cycle speaker display modes, filter one speaker and highlight another. Clear controls; reload. | All/change-only/none controls labels; filtering hides selected turns; highlighting dims others. No text is deleted. Track assumption badges separately from assigned names. |
| ED-14 | P0 | Add nonadjacent segments, reorder, remove one, rename story and reload. | Content, order and name persist; playback follows story order. Removed segments remain in source. Text edits can affect both source and active story. |
| ED-15 | P0 | Edit A, immediately switch A→B→A, then reload both. Repeat with delayed saves and distinct story content. | Each story retains its own latest saved content and name; A does not overwrite B. Verify persisted data. |
| ED-16 | P0 | Load uncached A slowly, then choose cached B before A finishes. | Late A response does not replace B. Selection, text, playback and save target all remain B. |
| ED-17 | P0 | Delete active story, confirm, switch/reload remaining stories; create a new one. | Deleted story is not recreated by a pending save. Others stay intact; new story is empty and uses a free slot. |
| ED-18 | P2 | Create ten stories; attempt an eleventh. Delete one and replace it. Also cancel a deletion. | UI enforces ten-story limit; deletion frees a slot; cancellation preserves content. |

## Save, recovery and restoration

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-19 | P0 | Make distinct source/story edits, confirm saves, then reopen and compare with baseline and intended result. | Each document contains exactly its committed edits. Verify contents, not only save responses. |
| ED-20 | P0 | Edit, then immediately reload/navigate away, hide tab or switch mobile apps. Repeat after confirmed save, then with failed save and network recovery. | Confirmed saves survive. Record local/server state, messages and loss timing for interrupted saves. Offline and app-termination recovery need an agreed contract. |
| ED-21 | P0 | Edit a test document. Cancel Restore Original, then confirm it. Reload source and stories. | Cancel changes nothing; confirm restores and persists source backup. Assess effects on active/inactive stories against agreed requirements. |

## Output verification

| ID | Priority | Action and data | Expected outcome / observation |
|---|---|---|---|
| ED-22 | P0 | Export source and differently ordered story to TXT/DOCX/JSON; copy their text. Open files and paste clipboard text into a plain-text editor. | Correct content, corrections and order. TXT/DOCX group speaker turns; JSON retains timing/metadata. DOCX opens; rich editor formatting is not carried into this export. |
| ED-23 | P0 | Edit text, leave edit mode, export SRT before autosave. Repeat with subtitle service unavailable. | Output includes current edits with valid, aligned cues. Check default and fallback paths separately; fallback uses segment-based cues and may differ. |
| ED-24 | P1 | Double-click SRT/REAPER export while running; retry after completion and failure. | No duplicate work for the same running action. Loading ends and retry works. Check downloaded files. |
| ED-25 | P0 | Export source and reordered story as REAPER ZIPs. Extract and open projects with bundled audio, including downloads from Safari. | Audio references resolve. Project/audio are usable, with correct content, timing and story order. Listen to the result. |

## Questions before execution

- **Subtitles:** confirm reading speed, line length, language and timing rules with the backend team; the web export code does not define them all. Test fallback separately.
- **Shared edits:** should renaming/editing affect inactive stories? Should restoring source reset any story?
- **Save failures:** agree messages, retries and recovery after navigation, disconnection or app termination.
- **Speaker suggestions:** when should AI assumption badges persist, change or disappear? Current edit paths differ.
- **Tolerances:** agree alignment and response-time limits by device/document size. Existing measured boundaries can be preserved exactly; new unmeasured splits require estimates.

In the [release record](regression-and-release.md), attach fixture IDs, before/after evidence, results and defects. Mark untested cases **Not run** and unresolved requirements **Blocked / clarification required**.
