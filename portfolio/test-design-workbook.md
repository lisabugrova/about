# Test design workbook — Audiodoc Studio

**Status: in progress · Test-design artifact, not an execution report.**

This workbook connects software test design techniques to observable functionality in **Audiodoc Studio**. It replaces the previous fictional story-search examples as the main demonstration of design thinking.

## Feature and technique map

| Observed feature | Test-design technique | Why this technique fits | Work sample |
| --- | --- | --- | --- |
| File-upload quantity limit (maximum 10 files) | **Equivalence Partitioning (EP), Boundary Value Analysis (BVA)** | Distinguish permitted/over-limit classes; test 9, 10, 11 around the boundary | [File upload and available time](audiodoc-studio-file-upload.md) |
| Per-file size limit (up to 1700 MB each) | **EP, BVA** | Test just below, at, and just above the exact byte limit (unit needs clarification) | [File upload and available time](audiodoc-studio-file-upload.md) |
| Insufficient processing time versus video length | **EP, Decision Table Testing** | A zero balance and 10-second video produce a 10-second shortfall; at 1 hour a 26:16 video is ready to upload | [Observed balance validation scenarios](audiodoc-studio-balance-validation.md) |
| Select file versus drag-and-drop | **Functional testing and alternate-input coverage** | Both UI pathways must respect the same validation and access constraints | [File upload and available time](audiodoc-studio-file-upload.md) |
| Minutes/hours selector, plus/minus, payment selection | **BVA + Decision Tables, after rule clarification** | Need min/max increments, ranges, pricing and state rules before producing precise oracles | Planned |
| Price tiers beginning at 300, 600 and 1200 minutes, as shown in the expanded UI | **BVA + Decision Tables, after rule clarification** | Rate changes are possible decision thresholds, but the exact pricing formula and boundaries are not yet confirmed | Planned |
| Upload / processing lifecycle | **State Transition Testing** | Observed blocked/ready states; upload, processing warning and finished result are tester-reported; failures/retry/cancel need testing | [Observed balance validation scenarios](audiodoc-studio-balance-validation.md) |
| Browser and device combinations | **Pairwise Testing, after confirming supported environments** | Optimize cross-platform combinations while preserving two-factor coverage | [Educational example only](pairwise-drag-drop.md) |

## Key confirmed business rule

**REQ-UPL-001 (refined):** A file **can be selected** when balance is insufficient, but upload/processing is blocked. The **0 sec / 10 sec video** screenshot shows *Insufficient balance* and *Not enough time, you need 10 seconds more*. The **1 hour / 26:16 video** screenshot shows *Ready to upload!* and *Start upload!*; successful processing, download and editor access are reported by the tester.

Case [TC-BAL-001 and TC-BAL-002](audiodoc-studio-balance-validation.md) separate these behaviors and sources. **TC-UPL-002** (zero-balance drag-and-drop entry specifically) remains **Not Run**; selecting a file does not establish the exact input mechanism used.

## Work items before execution

1. **Observed:** with zero balance, a selected file is shown with an insufficient-balance message. Confirm whether the agreed UX requires a direct top-up instruction/action in that error and whether click-to-select and drag-and-drop behave identically.
2. Confirm the exact scope of the 10-file limit and the exact bytes meant by 1700 MB.
3. Confirm supported formats and duration-to-balance calculation (including exact equality and rounding). A positive balance **alone** does not prove eligibility.
4. Execute tests in an approved environment with synthetic files; document actual outcomes and defects.
5. Add verified product or release evidence only if publication is authorized.

## Study foundation

- Lee Copeland, *A Practitioner's Guide to Software Test Design* — EP, BVA, decisions and combinations.
- Sviatoslav Kulikov, *Software Testing: Basic Course* — practical testing foundations and documentation.
- [ISTQB CTFL 4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf) — standardized vocabulary, test design and reporting.

[← Detailed Audiodoc Studio case](audiodoc-studio-file-upload.md) · [← Portfolio home](../README.md)
