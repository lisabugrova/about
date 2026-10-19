# Audiodoc Studio — File Upload Test Design

**Selected design sample.** The decimal-MB requirement assumption below is retained. The [separate implementation reference](../reference/audiodoc-studio-file-upload.md) uses 1700 MiB (1,782,579,200 bytes); confirm the agreed unit before execution.

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
- Prepare **9, 10 and 11 files**, named `count_01.mp4` through `count_11.mp4`, each **10 seconds long and approximately 2 MB**, and set the available time to **1 hour (3600 s)**. The largest group requires 110 seconds of media time; this isolates the count limit from the balance gate.
- Prepare three valid MP4 files sized **1,699,999,999**, **1,700,000,000** and **1,700,000,001 bytes** (using decimal MB). Video duration is not a parameter in this test; available processing time must cover each selected file.
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
| File size (decimal-MB test set) | 1,699,999,999; 1,700,000,000; 1,700,000,001 bytes | At/around the 1700 MB boundary |

**Unit convention:** this test uses a limit of **1700 MB = 1,700,000,000 bytes**. Verify the product's MB definition against requirements before execution.

## Test cases

### TC-UPL-001 — Select a valid video using the file picker

**Preconditions / test data:** Signed-in user; available processing time **1 hour (3600 s)**; empty upload queue; supported `sample_video_26m16s.mp4`, duration **00:26:16 (1576 s)**.

1. Open **Your documents**.
2. Click the **Upload files** area.
3. Select the video.
4. Inspect the selected-file row.

**Expected result:** The selected video appears with its duration; when its duration is covered by available processing time and other constraints, **Ready to upload!** and **Start upload!** are displayed.

### TC-UPL-002 — Select a file via drag-and-drop

**Preconditions / test data:** Signed-in user; **1 hour (3600 s)** available; empty queue; supported `sample_video_26m16s.mp4` (**00:26:16**); desktop browser and OS file manager support dragging files.

1. Drag a supported file from the OS file manager into the upload zone.
2. Release it over the upload area.
3. Inspect the queue and validation state.

**Expected result:** File selection and validation follow the same rules as file-picker selection; the file appears in the queue without unintended duplicate entries.

### TC-UPL-003 — Accept a selection at the file-count limit

**Preconditions / test data:** Available processing time **1 hour (3600 s)**; empty queue; **`count_01.mp4`–`count_10.mp4`**, each **10 s**, approximately **2 MB** and of a supported format.

1. Select **`count_01.mp4`–`count_10.mp4`** (exactly **10 files**).
2. Check the number of queued files and any validation messages.

**Expected result:** The advertised 10-file limit is not exceeded. Ten otherwise eligible files can be selected, subject to confirmation of the limit's scope and other restrictions.

### TC-UPL-004 — Reject an over-limit file count

**Preconditions / test data:** Available processing time **1 hour (3600 s)**; empty queue; **`count_01.mp4`–`count_11.mp4`**, each **10 s** and approximately **2 MB**.

1. Select **`count_01.mp4`–`count_11.mp4`** (exactly **11 files**) in one operation.
2. Inspect the queue and feedback.

**Expected result:** No more than the permitted maximum is accepted. The handling policy (reject all or accept up to 10) must match the agreed requirement.

### TC-UPL-005 — Validate the per-file size boundary

**Preconditions:** signed-in user; empty upload queue; three valid, playable MP4 files of a supported type. Available processing time covers each file. Size limit for this test: **1,700,000,000 bytes**.

**Test data:**

| File | Size | Boundary |
| --- | ---: | --- |
| A | **1,699,999,999 bytes** | 1 byte below |
| B | **1,700,000,000 bytes** | Exactly at the limit |
| C | **1,700,000,001 bytes** | 1 byte above |

1. Select file **A** and inspect the size-validation result.
2. Clear the queue, select file **B**, and inspect validation.
3. Clear the queue, select file **C**, and inspect validation.

**Expected result:** files **A** and **B** satisfy the file-size restriction; file **C** is rejected for exceeding the limit. Other upload restrictions must not affect size validation.

### TC-UPL-006 — Clear the selected-file queue

**Preconditions / test data:** Available processing time **1 hour (3600 s)**; selected `sample_video_10s.mp4` (**10 s**, supported format); **Clear and start over** visible.

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

