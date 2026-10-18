# Audiodoc Studio — File Upload and Available-Time Gate

**Artifact type:** test analysis and test design based on a real UI, with an additional workflow rule provided by a QA team member.  
**Target UI:** Audiodoc Studio, *Your documents* → *Upload files*.  
**Execution status:** **Not Run** — no test run, build/version, logs, or recorded outcomes are provided in this public artifact.  
**Public data policy:** no client files, production credentials, private logs, payment details, or confidential product requirements.

## 1. Test basis and source confidence

| ID | Test basis | Source | Confidence / limitation |
| --- | --- | --- | --- |
| REQ-UPL-001 | When **Available time = 0 seconds**, a user **cannot upload a file**. | QA clarification from the product context | Known rule; exact UI enforcement and messages not yet documented |
| UI-UPL-002 | Upload area offers **drag and drop** and **click-to-select**. | Shared Audiodoc Studio screenshot | Visible affordances; implementation details unknown |
| UI-UPL-003 | **Maximum 10 files**. | Shared Audiodoc Studio screenshot | Whether limit applies per selection, batch, or document inventory needs confirmation |
| UI-UPL-004 | **Up to 1700 MB each**. | Shared Audiodoc Studio screenshot | MB vs MiB and exact byte limit need confirmation |
| UI-PUR-001 | Available time shows **0 seconds** and purchase-time controls are displayed. | Shared expanded UI screenshot | The existence of controls is visible; full purchase behavior is not established |
| UI-PUR-002 | Example displayed quote: **30 minutes × 15 ₽ = 450 ₽**; tiers are shown starting at **300/600/1200 minutes** with displayed prices of **14/13/11 ₽ per minute**. | Shared expanded UI screenshot | Snapshot only; do not assume verified live pricing or inclusive/exclusive tier behavior |

**Important:** screenshot-visible text, a tester's statement about a rule, and an *executed* passed test are different kinds of evidence. This file documents the first two, not the third.

## 2. Test analysis: what is in and out of scope?

**In scope:** upload access when time is zero; both upload initiation methods; count limit; individual size limit; visible feedback and document-list updates.

**Out of scope until clarified:** allowed file extensions, whether positive time always permits upload, processing duration versus balance, exact pricing logic, payment processing, backend API behavior, and how files persist after refresh.

### Clarifications required before assigning further expected results

1. With **0 seconds**, is file selection disabled, or is a selection allowed but the upload rejected with an error?
2. Does a **positive** balance permit uploading any supported file, or must it cover the media duration?
3. Is the ten-file limit per one batch, for all queued files, or for all stored documents?
4. Is **1700 MB** calculated with base-10 megabytes or MiB?
5. What file formats and media lengths are accepted?
6. What should happen when a mixed batch contains both acceptable and unacceptable files?

## 3. Equivalence Partitioning (EP)

| Dimension | Partition | Test example | Oracle |
| --- | --- | --- | --- |
| Available time | Zero | 0 seconds | Upload **must not succeed** (REQ-UPL-001) |
| Available time | Positive | > 0 seconds | **TBD:** depends on additional processing/balance rules |
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
| BVA-BAL-00 | 0 seconds | Upload not permitted |
| BVA-BAL-01 | >0 seconds, e.g. 1 second | **Unknown:** cannot infer acceptance from the zero-balance rule |

Avoid asserting “1 second permits uploading a two-hour file.” The required balance may depend on audio duration or other conditions.

## 5. Partial decision table — upload eligibility

| Rule | Available time | File count | Per-file size | Upload method | Expected result |
| --- | --- | --- | --- | --- | --- |
| DT-01 | 0 seconds | Within limit | Within limit | Picker | **Do not complete upload**; exact UX TBD |
| DT-02 | 0 seconds | Within limit | Within limit | Drag-and-drop | **Do not complete upload**; exact UX TBD |
| DT-03 | Positive | Within limit | Within limit | Either | **TBD**: confirm duration, format, and policy |
| DT-04 | Positive | Exceeds 10 | Within limit | Either | Enforce count limit; exact batch handling TBD |
| DT-05 | Positive | Within limit | Exceeds 1700 MB | Either | Enforce per-file size limit; exact feedback TBD |
| DT-06 | 0 seconds | Exceeds 10 | Exceeds 1700 MB | Either | Upload prohibited; validation order/message TBD |

This is an **incomplete decision table**, intentionally reflecting missing requirements. Balance, validation priority, and post-payment behavior cannot yet be modelled as complete business rules.

## 6. Written test cases (not executed)

### TC-UPL-001 — Prevent file upload with zero available time via file picker

- **Technique:** EP + business-rule testing; negative functional.
- **Priority:** High (access condition for a core action).
- **Preconditions:** authenticated user; **Available time = 0 seconds**; a small file of supported format; initial document list or upload queue recorded.
- **Steps:**
  1. Open *Your documents*.
  2. Check that *Available time* displays **0 seconds**.
  3. Try to open the file chooser from *Upload files*.
  4. If the chooser opens, select a valid, under-limit file.
  5. Observe feedback and the document list/upload queue.
- **Expected:** the file does **not** become successfully uploaded; the zero-time restriction is enforced. Exact button state and message require specification.
- **Actual:** Not recorded.
- **Status:** **Not Run**.

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

## 7. Further test opportunities

- **Purchase-time widget:** boundary values and tariff transitions near 300, 600, 1200 minutes **after** verifying how pricing brackets are applied.
- **State transitions:** No time → Purchase initiated → Payment outcome → Time available → Upload possible, **only after** the actual workflow is confirmed.
- **Risk-based regression:** both upload entry points; no unintended upload with zero time; large-file feedback; queue/list consistency; refresh during upload.
- **Security:** verify authorization, server-side validation, no leaks of uploaded contents; use synthetic data and an approved test environment.

## 8. Reporting and traceability

| Test case | Related basis | Design status | Execution status |
| --- | --- | --- | --- |
| TC-UPL-001 | REQ-UPL-001, UI-UPL-002 | Draft | Not Run |
| TC-UPL-002 | REQ-UPL-001, UI-UPL-002 | Draft | Not Run |
| TC-UPL-003 | UI-UPL-003 | Draft; clarification needed | Not Run |
| TC-UPL-004 | UI-UPL-004 | Draft; clarification needed | Not Run |

Record build, environment, browser/OS, setup, execution date, actual result, evidence, and defect references **only after executing tests**. Never fill pass/fail results from UI screenshots alone.

## Methodology references

- Lee Copeland, *A Practitioner's Guide to Software Test Design*: selecting equivalence classes, boundary tests, decision rules, and combinations.
- Sviatoslav Kulikov, *Software Testing: Basic Course*: test documentation and fundamental testing workflow.
- [ISTQB CTFL syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf): test analysis, design, coverage, traceability, and execution reporting.

[← Portfolio home](../README.md)
