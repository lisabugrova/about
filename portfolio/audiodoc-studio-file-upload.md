# Audiodoc Studio — Upload and Media Validation

**Artifact type:** reference test design. Original test IDs TC-UPL-001–006 are retained; expanded checks are not recorded executions.

## Test basis

| Rule | Expected contract |
| --- | --- |
| UP-01 | File picker and drag-and-drop support the same selection rules. |
| UP-02 | The complete selected queue contains at most ten files. |
| UP-03 | Per-file limit is **1,782,579,200 bytes**, inclusive: 1700 MiB despite the interface label “MB”. |
| UP-04 | Supported extensions are checked case-insensitively; extension acceptance alone does not prove valid media content. |
| UP-05 | Files expose identity, duration/validation and processing state; the queue can be cleared. |
| UP-06 | Known duration is rounded upward to seconds; metadata failure can use a fallback/probe path. |
| UP-07 | Processing eligibility depends on available time and combined demand; see the balance design. |

## Preparation

Use an authorized account and disposable media. Set one hour of available time for small selection checks. Prepare eleven distinct 10-second files and verify their names and duration. Reset the queue between independent tests.

For size tests use **valid, independently inspectable** media at 1,782,579,199 / 1,782,579,200 / 1,782,579,201 bytes. Retain a fixture manifest with exact bytes and duration. Do not rename or truncate random bytes and call the result a valid MP4. If valid boundary fixtures are unavailable, mark the media-path check blocked; a mocked File object tests only the client boundary, not decoding or end-to-end processing.

## Cases

| ID | Priority | Action / data | Expected result |
| --- | --- | --- | --- |
| TC-UPL-001 | P0 | Choose valid 26:16 MP4 with 3600 s balance. | One intended file, correct duration, eligible state and start action. |
| TC-UPL-002 | P1 | Drag the same file from a desktop file manager into an empty queue. | Same validation as picker; one row for this one selection. |
| TC-UPL-003 | P1 | Select ten distinct 10-second files together. | Ten accepted rows; sufficient balance does not obscure the count check. |
| TC-UPL-004 | P1 | Select eleven distinct valid files together. | At most ten accepted; additional file rejected with count feedback. Verify which ten were retained. |
| TC-UPL-005 | P1 | Independently select each exact-size fixture below/at/above the binary limit. | Below and equal satisfy size validation; one byte above is rejected. Decoding and balance remain separate checks. |
| TC-UPL-006 | P1 | Select valid files, then use Clear and start over. | Queue returns to initial state; no stale row or start action; subsequent selection works. |
| TC-UPL-007 | P1 | Add six files, then five distinct files. | Queue stops at ten across both selections; accepted rows remain identifiable. |
| TC-UPL-008 | P1 | At ten files, try picker and drop; remove one through the available control and add a replacement. | Neither input path exceeds ten; removal frees one slot. |
| TC-UPL-009 | P1 | Select representative MP3/WAV/M4A/FLAC and MP4/MOV/WEBM; repeat with uppercase extensions. | Allowlisted names are selectable; record actual metadata/processing support per fixture, without extrapolating to every codec. |
| TC-UPL-010 | P1 | Mix a valid file, unsupported extension and oversized file. | Invalid inputs receive appropriate feedback; valid accepted input remains usable; count reflects accepted rows. |
| TC-UPL-011 | P1 | Choose malformed content with an allowed extension and a zero-byte file. | No crash or false completed result. Metadata/probe/processing failure is explicit and recoverable; verify agreed treatment of invalid duration. |
| TC-UPL-012 | P1 | Use a verified media fixture whose browser metadata fails, including an appropriate M4A/MP4 case. | Fallback/probe resolves duration or reports a failure; unknown duration is not silently treated as free processing. |
| TC-UPL-013 | P1 | Select a slow-metadata file, clear immediately, then select another. | Late completion does not modify the new selection or recreate a removed row. |
| TC-UPL-014 | P1 | Select two different files with the same name; also reselect the identical file. | Rows/states remain attached to the correct file. Confirm the product's duplicate-selection policy before classifying intentional duplicates as defects. |
| TC-UPL-015 | P2 | Use long names, Cyrillic, spaces and punctuation. | Filename remains understandable; no broken selection or inaccessible controls. |
| TC-UPL-016 | P1 | Choose explicit language, diarization/verbatim options and an available project; submit a small batch. | The chosen settings and destination apply to the intended files; verify resulting document configuration. |

## Analysis notes

The lower file-count boundary includes an empty queue and a single file: empty cannot begin useful processing, while one eligible file can. For cumulative limits, test both one selection and repeated additions.

A selection test is not a codec-compatibility result. Observe metadata, transfer, processing and final output as distinct stages. Compare the displayed size label with the implemented threshold and agree whether the wording should change; the binary threshold is the current reference, not an invented decimal requirement.

[Balance and processing](audiodoc-studio-balance-validation.md) · [Coverage](product-coverage.md) · [Profile](../README.md)

