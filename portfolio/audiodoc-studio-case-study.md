# Audiodoc Studio — Manual QA Case Study

**Project:** audio/video transcription and editing web application  
**Role represented:** manual QA  
**Focus of the original work samples:** upload validation and processing-time balance

## Product context

Audiodoc Studio turns uploaded media into a transcript that a user can review alongside audio, edit, organize into stories and export. The application also includes accounts, projects, document-sharing permissions, processing-time purchases and transaction history.

Quality involves more than a successful upload. The user needs the right document, usable text, recoverable edits, appropriate access and understandable processing costs.

## Selected contribution

The upload and balance work samples turn interface rules into a structured test basis:

- file picker and drag-and-drop paths;
- the maximum selected-file count and per-file size;
- the relationship between media duration and available processing time;
- readiness, processing and access to results;
- clearing the queue and re-evaluating eligibility.

Concrete files, balances and queue states make the checks repeatable. Requirement IDs connect the rule to the relevant tests.

## Decisions illustrated by the work

| Decision | Why it matters |
| --- | --- |
| Provide sufficient balance when checking file-count limits | Prevents a balance error from hiding a queue-limit failure. |
| Check immediately below, at and above a boundary | Exposes off-by-one behavior and unit interpretation. |
| Treat expected insufficient-balance feedback as valid behavior | Avoids reporting correct business validation as a defect. |
| Separate observed readiness from reported completion | Keeps the strength of evidence visible to a reviewer. |
| Identify untested transitions and ambiguous rules | Gives the next test cycle a concrete starting point. |

## Recorded outcomes

The existing execution record describes two scenarios:

1. A 10-second video with zero available time displayed insufficient balance and a 10-second shortage; the normal upload action was absent.
2. A 26:16 video with one hour available displayed readiness; the tester reported successful processing, a leave-page warning, download and editor access.

The original record does not include build, browser, date or screenshot attachments. These outcomes remain recorded at their original evidence level; the reference designs below do not add historical test results.

[Read the execution record](test-execution-report.md)

## Broader product reference

The repository now provides a structured reference for covering the complete user journey: account and consent → upload and processing → editing and saving → export and sharing → balance and history.

This reference is based on the reviewed product snapshot and is separate from the historical execution record. It illustrates how to expand a feature-focused test suite without treating unexecuted checks as passes.

[Product coverage](product-coverage.md) · [Test-design index](test-design-workbook.md) · [Profile](../README.md)

