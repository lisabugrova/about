# Audiodoc Studio — File Upload Test Design

**Feature:** Your documents → Upload files  
**Test type:** Functional, positive and negative  
**Techniques:** Equivalence Partitioning (EP), Boundary Value Analysis (BVA), input-path testing  
**Objective:** Verify file selection, queue validation, upload limits and reset behavior.

## Test basis

| ID | Requirement / interface rule |
| --- | --- |
| UP-01 | Users can select files using the file picker or drag files into the upload area. |
| UP-02 | The upload area states: **Maximum 10 files**. |
| UP-03 | The upload area states: **Up to 1700 MB each**. |
| UP-04 | Selected files display their name, duration and validation state. |
| UP-05 | The interface provides **Clear and start over**. |
| UP-06 | Starting an upload is controlled by available processing time; see [Balance Validation Test Design](audiodoc-studio-balance-validation.md). |

## Test-data preparation

- Use supported, non-sensitive media files. Confirm supported formats before execution.
- Prepare **9, 10 and 11 files** with an individual size below the limit; ensure the account balance is sufficient for the combined media duration so count validation can be isolated.
- Prepare files with sizes **L − 1 byte, L and L + 1 byte**, where **L** is the confirmed byte value of 1700 MB.
- Record the initial document count and reset the queue between independent scenarios.
- For drag-and-drop checks, use a supported desktop browser and a local file.

## Equivalence classes

| Input | Valid / relevant class | Invalid / alternative class |
| --- | --- | --- |
| Number of files | 1–10 | 11+ |
| Size per file | Within stated maximum | Above stated maximum |
| Input path | File picker | Drag and drop (alternative supported path) |
| Current queue | Empty | Contains selected files |

## Boundary values

| Parameter | Values to test | Purpose |
| --- | --- | --- |
| File count | 0, 1, 9, 10, 11 | Empty input and 10-file limit |
| File size | L − 1 byte, L, L + 1 byte | Exact per-file size limit |

**Clarify before execution:** whether 10 is a per-batch or total-queue limit; whether MB is decimal or binary; how an invalid file in a mixed selection is handled.

## Test cases

### TC-UPL-001 — Select a valid video using the file picker

**Preconditions:** Signed-in user, sufficient available processing time for the video, empty upload queue; supported 26 min 16 sec video.

1. Open **Your documents**.
2. Click the **Upload files** area.
3. Select the video.
4. Inspect the selected-file row.

**Expected result:** The selected video appears with its duration; when its duration is covered by available processing time and other constraints, **Ready to upload!** and **Start upload!** are displayed.

### TC-UPL-002 — Select a file via drag-and-drop

**Preconditions:** Same as TC-UPL-001; OS/file manager supports dragging files.

1. Drag a supported file from the OS file manager into the upload zone.
2. Release it over the upload area.
3. Inspect the queue and validation state.

**Expected result:** File selection and validation follow the same rules as file-picker selection; the file appears in the queue without unintended duplicate entries.

### TC-UPL-003 — Accept a selection at the file-count limit

**Preconditions:** Sufficient available processing time; 10 valid files below the size limit; empty queue.

1. Select exactly **10** files.
2. Check the number of queued files and any validation messages.

**Expected result:** The advertised 10-file limit is not exceeded. Ten otherwise eligible files can be selected, subject to confirmation of the limit's scope and other restrictions.

### TC-UPL-004 — Reject an over-limit file count

**Preconditions:** Same setup as TC-UPL-003, with **11** files.

1. Select **11** files in one operation.
2. Inspect the queue and feedback.

**Expected result:** No more than the permitted maximum is accepted. The handling policy (reject all or accept up to 10) must match the agreed requirement.

### TC-UPL-005 — Validate the per-file size boundary

**Preconditions:** Sufficient time, supported file type, exact byte limit **L** confirmed.

1. Select a file of size **L − 1 byte**.
2. Reset the queue and select a file of size **L**.
3. Reset the queue and select a file of size **L + 1 byte**.

**Expected result:** The first two sizes satisfy the advertised size limit; the over-limit file is not accepted as valid. Actual acceptance still depends on other file requirements.

### TC-UPL-006 — Clear the selected-file queue

**Preconditions:** One or more files selected; **Clear and start over** visible.

1. Click **Clear and start over**.
2. Inspect the file queue and available actions.

**Expected result:** Selected-file entries are cleared and the upload form returns to its initial selection state.

## Requirements traceability

| Requirement | Test cases |
| --- | --- |
| UP-01 | TC-UPL-001, TC-UPL-002 |
| UP-02 | TC-UPL-003, TC-UPL-004 |
| UP-03 | TC-UPL-005 |
| UP-04 | TC-UPL-001, TC-UPL-002 |
| UP-05 | TC-UPL-006 |
| UP-06 | [Balance Validation Test Design](audiodoc-studio-balance-validation.md) |

[← Test design overview](test-design-workbook.md) · [← Portfolio](../README.md)
