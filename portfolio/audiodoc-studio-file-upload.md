# Audiodoc Studio — Upload and Media Validation

**Test design.** Original IDs: TC-UPL-001–006; the remaining cases extend the plan.

## Rules

| Rule | Expected behavior |
| --- | --- |
| UP-01 | File picker and drag-and-drop support the same selection rules. |
| UP-02 | The queue holds at most ten files. |
| UP-03 | Per-file limit is **1,782,579,200 bytes**, inclusive: 1700 MiB despite the interface label “MB”. |
| UP-04 | Extension checks ignore case. An accepted extension does not guarantee valid media. |
| UP-05 | Each row identifies the file and shows duration, validation and processing state. The queue can be cleared. |
| UP-06 | Known duration rounds up to whole seconds. If metadata cannot be read, a fallback or server probe may determine it. |
| UP-07 | Available time must cover the combined demand; see the balance checks. |

## Preparation

Use a test account with one hour of available time and eleven distinct 10-second files. Verify their names and duration. Clear the queue between independent tests.

For size tests, prepare valid media at 1,782,579,199 / 1,782,579,200 / 1,782,579,201 bytes. Record independently verified size and duration. Renamed or truncated random data is not valid MP4. If these files are unavailable, mark the end-to-end check blocked: a mocked File object tests the client size limit, not decoding or processing.

## Cases

| ID | Priority | Action / data | Expected result |
| --- | --- | --- | --- |
| TC-UPL-001 | P0 | Choose valid 26:16 MP4 with 3600 s balance. | Correct file and duration; processing can start. |
| TC-UPL-002 | P1 | Drag the same file from a desktop file manager into an empty queue. | Same checks as the picker; one row added. |
| TC-UPL-003 | P1 | Select ten distinct 10-second files together. | Ten rows accepted. |
| TC-UPL-004 | P1 | Select eleven distinct valid files together. | At most ten accepted; count-limit message shown. Check which ten remain. |
| TC-UPL-005 | P1 | Separately select the size fixtures below, at and above the binary limit. | Below and equal pass the size check; one byte above fails. Check decoding and balance separately. |
| TC-UPL-006 | P1 | Select valid files, use Clear, then select again. | No old rows or active start action remain; new selection works. |
| TC-UPL-007 | P1 | Add six files, then five different files. | Queue stops at ten; retained files are identifiable. |
| TC-UPL-008 | P1 | At ten files, try picker and drop; remove one and add a replacement. | Neither path exceeds ten; removal frees one slot. |
| TC-UPL-009 | P1 | Select MP3/WAV/M4A/FLAC and MP4/MOV/WEBM samples; repeat with uppercase extensions. | Allowed extensions are accepted. Record metadata and processing support for each file, not every codec in that format. |
| TC-UPL-010 | P1 | Mix a valid file, unsupported extension and oversized file. | Invalid files get relevant errors; the valid file remains usable; count includes accepted rows only. |
| TC-UPL-011 | P1 | Choose malformed content with an allowed extension and a zero-byte file. | No crash or false completion. Metadata/probe/processing errors allow recovery; invalid duration follows the agreed rule. |
| TC-UPL-012 | P1 | Use valid media whose browser metadata fails, including an M4A/MP4 example. | Fallback/probe finds duration or reports failure; unknown duration does not allow free processing. |
| TC-UPL-013 | P1 | Select a slow-metadata file, clear immediately, then select another. | Late metadata does not change the new selection or restore a removed row. |
| TC-UPL-014 | P1 | Select different files with the same name; also reselect the same file. | Rows and states belong to the correct files. Check the duplicate-selection rule before reporting duplicates as defects. |
| TC-UPL-015 | P2 | Use long names, Cyrillic, spaces and punctuation. | Names remain readable; selection and controls work. |
| TC-UPL-016 | P1 | Choose language, diarization/verbatim options and a project; submit a small batch. | Resulting documents use the selected settings and project. |

## Boundary checks

Check empty and single-file queues: an empty queue cannot start processing; one valid, affordable file can. Test the count limit with both a single selection and repeated additions.

Check metadata, transfer, processing and output separately. Use the implemented binary size limit; discuss the “MB” label separately from threshold correctness.

[Balance and processing](audiodoc-studio-balance-validation.md) · [Coverage](product-coverage.md) · [Profile](../README.md)

