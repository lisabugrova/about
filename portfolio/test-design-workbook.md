# Test design workbook — Audiodoc Studio

**Status: in progress · Test-design artifact, not an execution report.**

This workbook connects software test design techniques to observable functionality in **Audiodoc Studio**. It replaces the previous fictional story-search examples as the main demonstration of design thinking.

## Feature and technique map

| Observed feature | Test-design technique | Why this technique fits | Work sample |
| --- | --- | --- | --- |
| File-upload quantity limit (maximum 10 files) | **Equivalence Partitioning (EP), Boundary Value Analysis (BVA)** | Distinguish permitted/over-limit classes; test 9, 10, 11 around the boundary | [File upload and available time](audiodoc-studio-file-upload.md) |
| Per-file size limit (up to 1700 MB each) | **EP, BVA** | Test just below, at, and just above the exact byte limit (unit needs clarification) | [File upload and available time](audiodoc-studio-file-upload.md) |
| Upload restricted at **0 seconds** of available time | **EP, Decision Table Testing** | Availability is a business-rule condition that must be enforced through both upload entry points | [File upload and available time](audiodoc-studio-file-upload.md) |
| Select file versus drag-and-drop | **Functional testing and alternate-input coverage** | Both UI pathways must respect the same validation and access constraints | [File upload and available time](audiodoc-studio-file-upload.md) |
| Minutes/hours selector, plus/minus, payment selection | **BVA + Decision Tables, after rule clarification** | Need min/max increments, ranges, pricing and state rules before producing precise oracles | Planned |
| Price tiers beginning at 300, 600 and 1200 minutes, as shown in the expanded UI | **BVA + Decision Tables, after rule clarification** | Rate changes are possible decision thresholds, but the exact pricing formula and boundaries are not yet confirmed | Planned |
| Upload / processing lifecycle | **State Transition Testing, after observing real states** | Upload, progress, completed/failed/canceled may involve transitions; specific states are not asserted without evidence | Planned |
| Browser and device combinations | **Pairwise Testing, after confirming supported environments** | Optimize cross-platform combinations while preserving two-factor coverage | [Educational example only](pairwise-drag-drop.md) |

## Key confirmed business rule

**REQ-UPL-001:** When `Available time = 0 seconds`, a user **cannot upload a file**. This is a QA clarification about expected product behavior; it has **not been marked Pass** in this portfolio.

The first draft tests are **TC-UPL-001** (file picker with zero balance) and **TC-UPL-002** (drag-and-drop with zero balance). Both have **Not Run** status until test execution is documented.

## Work items before execution

1. Clarify **how** the zero-balance restriction is presented to users (disabled chooser, error upon selection, etc.).
2. Confirm the exact scope of the 10-file limit and the exact bytes meant by 1700 MB.
3. Confirm supported formats and whether **positive** available time guarantees upload eligibility.
4. Execute tests in an approved environment with synthetic files; document actual outcomes and defects.
5. Add verified product or release evidence only if publication is authorized.

## Study foundation

- Lee Copeland, *A Practitioner's Guide to Software Test Design* — EP, BVA, decisions and combinations.
- Sviatoslav Kulikov, *Software Testing: Basic Course* — practical testing foundations and documentation.
- [ISTQB CTFL 4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf) — standardized vocabulary, test design and reporting.

[← Detailed Audiodoc Studio case](audiodoc-studio-file-upload.md) · [← Portfolio home](../README.md)
