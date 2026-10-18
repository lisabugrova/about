# Audiodoc Studio — File Upload and Available-Time Gate

**Artifact type:** test analysis and test design based on a real UI, with an additional workflow rule provided by a QA team member.  
**Target UI:** Audiodoc Studio, *Your documents* → *Upload files*.  
**Evidence status:** Screenshot-based observation of a blocked 10-second video at 0 available seconds; screenshot-based ready state of a 26:16 video at 1 hour; successful upload/processing and output reported by the tester. Other test cases are **Not Run**. Build/browser/version and full execution logs are not recorded.  
**Public data policy:** no client files, production credentials, private logs, payment details, or confidential product requirements.

## 1. Test basis and source confidence

| ID | Test basis | Source | Confidence / limitation |
| --- | --- | --- | --- |
| REQ-UPL-001 | If processing time is insufficient, a file can be **selected**, but upload/processing is **blocked** with an insufficient-balance error. | Tester clarification + zero-balance screenshot | 0 seconds available / 10-second video: shortage of 10 seconds is shown; broader duration thresholds need verification |
| UI-UPL-002 | Upload area offers **drag and drop** and **click-to-select**. | Shared Audiodoc Studio screenshot | Visible affordances; implementation details unknown |
| UI-UPL-003 | **Maximum 10 files**. | Shared Audiodoc Studio screenshot | Whether limit applies per selection, batch, or document inventory needs confirmation |
| UI-UPL-004 | **Up to 1700 MB each**. | Shared Audiodoc Studio screenshot | MB vs MiB and exact byte limit need confirmation |
| UI-PUR-001 | Available time shows **0 seconds** and purchase-time controls are displayed. | Shared expanded UI screenshot | The existence of controls is visible; full purchase behavior is not established |
| UI-PUR-002 | Example displayed quote: **30 minutes × 15 ₽ = 450 ₽**; tiers are shown starting at **300/600/1200 minutes** with displayed prices of **14/13/11 ₽ per minute**. | Shared expanded UI screenshot | Snapshot only; do not assume verified live pricing or inclusive/exclusive tier behavior |

**Important:** distinguish screenshot-visible behavior, tester-reported follow-through, and a fully documented test execution. See [two observed balance scenarios and a potential top-up UX finding](audiodoc-studio-balance-validation.md).

## 2. Test analysis: what is in and out of scope?

**In scope:** selection followed by balance validation when time is zero; observed sufficient-balance ready state; both upload initiation methods; count limit; individual size limit; visible feedback and document-list updates.

**Out of scope until clarified:** allowed file extensions, exact balance-versus-duration comparison/rounding, whether arbitrary positive time permits processing, exact pricing logic, payment processing, backend API behavior, and how files persist after refresh.

### Clarifications required before assigning further expected results

1. **Observed:** at **0 seconds**, the file can be selected; the file row shows **Insufficient balance**, with the precise shortage for the tested 10-second video. Confirm whether the product specification requires an explicit **purchase/top-up instruction** in the error itself.
2. What are the exact comparison and rounding rules for time balance versus media duration? A **1-hour balance / 26:16 video** shows a ready state and was reported to process successfully; this does not prove *every* positive balance is sufficient.
3. Is the ten-file limit per one batch, for all queued files, or for all stored documents?
4. Is **1700 MB** calculated with base-10 megabytes or MiB?
5. What file formats and media lengths are accepted?
6. What should happen when a mixed batch contains both acceptable and unacceptable files?

## 3. Equivalence Partitioning (EP)

| Dimension | Partition | Test example | Oracle |
| --- | --- | --- | --- |
| Available time relative to video | Insufficient | 0 seconds available / 10-second video | File selected; **Insufficient balance**, shortage 10 seconds; cannot proceed |
| Available time relative to video | Sufficient, observed case | 1 hour available / 26:16 video | **Ready to upload!** and enabled **Start upload!**, processing/completion reported by tester |
| Available time relative to video | Boundary / uncertain | Balance approximately equal to video duration | **TBD:** exact rules and rounding must be verified |
| File count | Inside stated maximum | 1–10 files | Count alone does not exceed the advertised limit; other rules still apply |
| File count | Above stated maximum | 11+ files | Operation must not result in violating the displayed maximum; handling TBD |
| Individual file size | At/below stated maximum | ≤1700 MB | Size alone is within advertised limit, assuming supported type |
| Individual file size | Above stated maximum | >1700 MB | Must not accept an over-limit file as valid; handling TBD |
| Input method | Drag and drop | Drop file onto upload area | Must respect the same access/validation rules |
| Input method | File chooser | Click upload area and select file | Must respect the same access/validation rules |

**EP is a design choice, not an assertion** that all items within a partition behave identically in production.

## 4. Boundary Value Analysis (BVA)

### File count: maximum = 10

| Case | File count | Design intent |
| --- | ---: | --- |
| BVA-COUNT-00 | 0 | No selection / empty-input handling |
| BVA-COUNT-01 | 1 | Smallest practical non-empty selection |
| BVA-COUNT-09 | 9 | Below boundary |
| BVA-COUNT-10 | 10 | At boundary |
| BVA-COUNT-11 | 11 | Above boundary; violation must be prevented |

### File size: maximum = 1700 MB per file

| Case | Proposed size | Design intent |
| --- | --- | --- |
| BVA-SIZE-BELOW | Limit − 1 byte | Just below boundary |
| BVA-SIZE-AT | Exact limit | At boundary |
| BVA-SIZE-ABOVE | Limit + 1 byte | Just above boundary |

**Setup dependency:** first confirm the exact number of bytes meant by “1700 MB”. Do not claim precise BVA until test files are generated to that exact convention. Large-file testing may require a safe, approved test environment.

### Available-time boundary

| Case | Balance | Known outcome |
| --- | --- | --- |
| BVA-BAL-00 | 0 seconds; selected video 10 seconds | **Observed:** file is selectable but cannot proceed; shortage of 10 seconds |
| BVA-BAL-SHORT | 1 second less than the selected video's required time | **Not Run:** proposed rejection; need precise rule |
| BVA-BAL-EQUAL | Exactly the required video-processing time | **Not Run:** proposed boundary; need equality and rounding rule |
| BVA-BAL-LONG | 1 second more than the selected video's required time | **Not Run:** proposed acceptance; additional constraints still apply |

Do not infer that any positive balance always permits processing. See the [balance validation case](audiodoc-studio-balance-validation.md).

## 5. Partial decision table — upload eligibility

| Rule | Available time | File count | Per-file size | Upload method | Expected result |
| --- | --- | --- | --- | --- | --- |
| DT-01 | 0 seconds; video 10 sec | Within limit | Within limit | Picker / file selection | **Observed:** file selected; insufficient-balance error; shortfall 10 sec; no Start upload action |
| DT-02 | 0 seconds; video 10 sec | Within limit | Within limit | Drag-and-drop | **Not Run:** expected same balance check; UI feedback to verify |
| DT-03 | 1 hour; video 26:16 | Within limit | Within limit | Selection method not recorded | **Observed:** Ready to upload; subsequent processing and output **tester-reported** |
| DT-04 | Positive | Exceeds 10 | Within limit | Either | Enforce count limit; exact batch handling TBD |
| DT-05 | Positive | Within limit | Exceeds 1700 MB | Either | Enforce per-file size limit; exact feedback TBD |
| DT-06 | 0 seconds | Exceeds 10 | Exceeds 1700 MB | Either | Upload prohibited; validation order/message TBD |

This is an **incomplete decision table**, intentionally reflecting missing requirements. Balance, validation priority, and post-payment behavior cannot yet be modelled as complete business rules.

## 6. Written test cases (not executed)

### TC-UPL-001 — Show insufficient-balance error after selecting a file with zero available time

- **Technique:** EP + business-rule testing; negative functional.
- **Priority:** High (access condition for a core action).
- **Preconditions:** authenticated user; **Available time = 0 seconds**; a small file of supported format; initial document list or upload queue recorded.
- **Steps:**
  1. Open *Your documents*.
  2. Check that *Available time* displays **0 seconds**.
  3. Select a **10-second** supported video using the upload area.
  4. Observe the selected file row, shortage message, and availability of *Start upload!*.
  5. Confirm that processing/upload cannot begin with insufficient time.
- **Expected:** user can select the file, but the system blocks starting processing/upload and explains insufficient balance and a 10-second shortage. An explicit instruction to purchase additional time is a **proposed UX acceptance criterion** that still needs confirmation.
- **Actual (screenshot):** *Insufficient balance*, duration *10 seconds*, and *Not enough time, you need 10 seconds more*. No *Start upload!* button appears; purchase-time controls exist elsewhere on the page.
- **Status:** **Observed — core insufficient-balance behavior matches the expected rule**; lack of an explicit top-up instruction in the error is a **potential UX issue**, not a confirmed defect. Method of file selection is not directly established by the screenshot; the steps above specify one reproducible path.
- **Full test case and paired successful scenario:** [balance validation report](audiodoc-studio-balance-validation.md).

### TC-UPL-002 — Prevent file upload with zero available time via drag-and-drop

- **Technique:** EP + alternate input path.
- **Preconditions:** same as TC-UPL-001.
- **Steps:** open the page with zero time; drag a valid small file from the OS into the visible drop zone; release; observe the upload state and document list.
- **Expected:** no successful file upload (REQ-UPL-001), regardless of input method. Exact feedback TBD.
- **Actual:** Not recorded.
- **Status:** **Not Run**.

### TC-UPL-003 — Enforce maximum of ten files

- **Technique:** BVA + EP.
- **Preconditions:** test account/environment with upload access **confirmed**; permitted test-file formats/sizes; known initial queue state.
- **Steps:** attempt separate test selections of 9, 10 and 11 files; reset test state between attempts; record which files are accepted.
- **Expected:** the system respects the advertised **maximum 10 files**. Whether overflow causes partial acceptance or whole-batch rejection is **TBD**.
- **Actual:** Not recorded.
- **Status:** **Not Run**.

### TC-UPL-004 — Enforce individual file-size ceiling

- **Technique:** BVA.
- **Preconditions:** upload access confirmed; supported file type; exact byte limit and safe test-data strategy confirmed.
- **Steps:** test files of (limit − 1 byte), exact limit, and (limit + 1 byte); record UI and upload outcome.
- **Expected:** the above-limit file is rejected as valid input. Precise handling of other tests depends on format, balance, and product requirements.
- **Actual:** Not recorded.
- **Status:** **Not Run**.

### TC-UPL-005 — Select, upload and process a 26:16 video with 1-hour balance

- **Technique:** EP; positive end-to-end scenario and observed state transitions.
- **Preconditions:** account shows **1 hour** of available time; supported **26-minute 16-second** video.
- **Steps:** select video; verify *Ready to upload!* and *Start upload!*; start upload; observe processing warning; wait for result; use download and open-in-editor actions.
- **Expected:** selection is allowed, processing succeeds and both result actions work.
- **Actual:** ready state and button are visible in a screenshot; the tester **reports** successful processing, warning not to leave the page, and functioning download/editor actions.
- **Status:** **Pass — tester-reported**; ready state screenshot-confirmed, completion not accompanied by separate public evidence.
- **Detailed record:** [balance validation report](audiodoc-studio-balance-validation.md).

## 7. Further test opportunities

- **Purchase-time widget:** boundary values and tariff transitions near 300, 600, 1200 minutes **after** verifying how pricing brackets are applied.
- **State transitions:** No time → Purchase initiated → Payment outcome → Time available → Upload possible, **only after** the actual workflow is confirmed.
- **Risk-based regression:** both upload entry points; no unintended upload with zero time; large-file feedback; queue/list consistency; refresh during upload.
- **Security:** verify authorization, server-side validation, no leaks of uploaded contents; use synthetic data and an approved test environment.

## 8. Reporting and traceability

| Test case | Related basis | Design status | Execution status |
| --- | --- | --- | --- |
| TC-UPL-001 | REQ-UPL-001, UI-UPL-002 | Observation written up; exact entry method TBD | Observed: balance block; UX follow-up |
| TC-UPL-002 | REQ-UPL-001, UI-UPL-002 | Draft | Not Run |
| TC-UPL-003 | UI-UPL-003 | Draft; clarification needed | Not Run |
| TC-UPL-004 | UI-UPL-004 | Draft; clarification needed | Not Run |
| TC-UPL-005 | REQ-UPL-001 and positive-balance observation | Written up | Tester-reported Pass; ready state screenshot-backed |

Record build, environment, browser/OS, setup, test date, actual result, evidence and defect references for a reproducible execution record. The current **tester-reported** success is not equivalent to an independently verified run. A screenshot of a ready state does not prove completion.

## Methodology references

- Lee Copeland, *A Practitioner's Guide to Software Test Design*: selecting equivalence classes, boundary tests, decision rules, and combinations.
- Sviatoslav Kulikov, *Software Testing: Basic Course*: test documentation and fundamental testing workflow.
- [ISTQB CTFL syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf): test analysis, design, coverage, traceability, and execution reporting.

[← Portfolio home](../README.md)
