# Audiodoc Studio — Balance Validation & Processing Test Design

**Feature:** Processing-time balance → file selection → upload → results  
**Test type:** Functional, positive and negative  
**Techniques:** Equivalence Partitioning (EP), Decision Table Testing, Boundary Value Analysis (BVA), State Transition Testing  
**Objective:** Verify upload eligibility against video duration and the main successful-processing journey.

## Test basis

| ID | Business rule / expected behavior |
| --- | --- |
| BAL-01 | A file may be selected even when **Available time** is insufficient. |
| BAL-02 | When available time is insufficient, the file is marked **Insufficient balance**; the UI indicates missing time and upload cannot start. |
| BAL-03 | When time is sufficient, the selected file is marked **Ready to upload!** and **Start upload!** is available. |
| BAL-04 | After **Start upload!**, the user is advised not to leave the page while upload/processing continues. |
| BAL-05 | After successful processing, users can download the result or open it in the editor. |

## Test data

| Data set | Available time | Video duration | Purpose |
| --- | --- | --- | --- |
| BAL-D01 | **0 seconds** | **10 seconds** | Insufficient balance |
| BAL-D02 | **1 hour** (3600 s) | **26 min 16 s** (1576 s) | Sufficient balance; successful journey |
| BAL-D03 | **9 seconds** | **10 seconds** | One second below video duration |
| BAL-D04 | **10 seconds** | **10 seconds** | Balance equals video duration |
| BAL-D05 | **11 seconds** | **10 seconds** | One second above video duration |

**Test files:** `sample_video_10s.mp4` — 10 s (BAL-D01, BAL-D03–05); `sample_video_26m16s.mp4` — 26 min 16 s (BAL-D02). [Test data specification](test-data-catalog.md).

## Equivalence classes

| Class | Balance relative to required media time | Expected behavior |
| --- | --- | --- |
| EP-BAL-01 | Insufficient | File can be selected; upload is blocked; shortage displayed |
| EP-BAL-02 | Sufficient | File can reach **Ready to upload!**; upload action available |
| EP-BAL-03 | Balance **10 seconds**, video **10 seconds** | Apply specified time comparison and rounding policy |

## Decision table

| Rule | Available time | Video duration | Expected system state |
| --- | --- | --- | --- |
| DT-BAL-01 | 0 s | 10 s | **Insufficient balance**, need 10 more seconds; cannot start |
| DT-BAL-02 | 3600 s | 1576 s | **Ready to upload!**; **Start upload!** available |
| DT-BAL-03 | 9 s | 10 s | Expected shortage **1 s** if time is charged per second; verify rounding |
| DT-BAL-04 | 10 s | 10 s | Equality boundary: verify approved comparison rule |
| DT-BAL-05 | 11 s | 10 s | Sufficient if time is charged per second; verify additional constraints |

## Test cases

### TC-BAL-001 — Insufficient balance prevents upload

**Preconditions / test data:** Signed-in user; **Available time = 0 seconds**; supported video **`sample_video_10s.mp4`**, duration **00:00:10**; empty selected-file queue.

1. Open **Your documents**.
2. Confirm that **Available time** is **0 seconds**.
3. Select the 10-second video.
4. Inspect the validation message and the upload action.

**Expected result:** The file appears in the selection list with **Insufficient balance**. The message indicates that **10 seconds** of additional time are needed. **Start upload!** is not available; processing does not start. The user can identify that additional processing time is required.

### TC-BAL-002 — Process a video with sufficient balance

**Preconditions / test data:** Signed-in user; **Available time = 1 hour (3600 s)**; supported video **`sample_video_26m16s.mp4`**, duration **00:26:16 (1576 s)**; empty selected-file queue.

1. Open **Your documents** and select the video.
2. Check the file status and available actions.
3. Click **Start upload!**.
4. Observe upload/processing and the navigation warning.
5. Wait for processing to finish.
6. Download the result and open it in the editor.

**Expected result:** **Ready to upload!** is shown; **Start upload!** is available; upload/processing proceeds with a warning not to leave the page; both result actions work on completion.

### TC-BAL-003 — Check the eligibility boundary

**Preconditions / test data:** Supported **`sample_video_10s.mp4`**, duration **00:00:10**; separate account states with available time **9 s**, **10 s** and **11 s**. Clear the file queue before each attempt.

1. Set the available time to **9 seconds** and select **`sample_video_10s.mp4`** (00:00:10).
2. Record whether **Insufficient balance** or **Ready to upload!** appears, the reported shortage and the availability of **Start upload!**.
3. Clear the selection, set the available time to **10 seconds**, and select the same file; record the result.
4. Repeat with **11 seconds** available; compare each outcome against the confirmed calculation/rounding rule.

**Expected result:** Availability, shortage and permitted actions match the confirmed balance-calculation rule without off-by-one errors.

### TC-BAL-004 — Validate the incomplete-balance workflow

**Preconditions / test data:** Signed-in account with **0 seconds** available; **`sample_video_10s.mp4`** (00:00:10) selected and marked **Insufficient balance**; authorized test balance adjustment from **0 seconds** to **1 hour** available in the test environment.

1. Check that the upload/processing action is unavailable.
2. Locate the **Purchase time** controls.
3. In an approved test environment, increase available time from **0 seconds to 1 hour (3600 seconds)** without making an unauthorized real payment.
4. Recheck the same **10-second** video without changing the file.

**Expected result:** The user cannot start processing while time is insufficient. After sufficient time becomes available, the file becomes eligible according to the approved balance-refresh behavior.

## State-transition coverage

| Current state | Event / condition | Expected next state |
| --- | --- | --- |
| No file selected | Select valid file, balance insufficient | **Insufficient balance** |
| No file selected | Select valid file, balance sufficient | **Ready to upload!** |
| Ready to upload | Click **Start upload!** | Upload/processing in progress |
| Upload/processing | Processing succeeds | Completed |
| Completed | Click **Download** or **Open in editor** | Result accessible |

**Additional transitions to specify:** network failure, processing error, retry, removal from queue, navigation away and balance changes after selection.

## Traceability

| Requirement | Test cases |
| --- | --- |
| BAL-01, BAL-02 | TC-BAL-001, TC-BAL-003 |
| BAL-03 | TC-BAL-002, TC-BAL-003, TC-BAL-004 |
| BAL-04, BAL-05 | TC-BAL-002 |
| Balance update / refresh behavior (0 s → 3600 s, selected 10 s file) | TC-BAL-004 |

[← Test design overview](test-design-workbook.md) · [← Portfolio](../README.md)
