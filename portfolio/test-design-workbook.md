# QA Portfolio — Reading Guide

## Artifact types

The original portfolio supplies the upload/balance work samples and the recorded results. The expanded and revised reference designs were prepared collaboratively with the project owner and AI assistance, using a review of the product source. They are not presented as Elizaveta's independently authored historical work. The illustrative defect and release examples are fictional.

| Type | Meaning | Where |
| --- | --- | --- |
| Recorded execution | Results retained from the existing portfolio, with original evidence limitations | [Execution record](test-execution-report.md) |
| Reference test design | Prepared checks, fixtures, expectations and risk priorities; no execution result implied | Feature designs and coverage map |
| Illustrative example | A clearly labeled fictional scenario showing reporting technique | Worked defect and release examples |
| Process reference | A reusable way to organize QA work | Regression selection and handover |

**Reference baseline:** Audiodoc Studio source snapshot reviewed on 18 October 2026. A deployed build may differ. Confirm the target build and supported environments before executing. The reference covers the reviewed application's user-facing domains, not every possible input or the internals of its separate transcription service.

## Reading paths

- **Recruiter:** [CV](../CV.md) → [case study](audiodoc-studio-case-study.md).
- **QA reviewer:** [coverage map](product-coverage.md) → [upload](audiodoc-studio-file-upload.md) → [balance](audiodoc-studio-balance-validation.md) → [execution record](test-execution-report.md).
- **Delivery team:** [regression and release](regression-and-release.md) → [defect example](defect-report-template.md) → [handover](qa-handover-playbook.md).

## Coverage and techniques

| Artifact | Main techniques |
| --- | --- |
| [Upload and media validation](audiodoc-studio-file-upload.md) | Equivalence classes, exact boundaries, cumulative selection, input-path equivalence |
| [Balance and processing](audiodoc-studio-balance-validation.md) | Decision rules, duration rounding, aggregated demand, states and recovery |
| [Editor and output](editor-and-output.md) | Data persistence, ordering, audio/text correspondence, compatibility between document generations |
| [Accounts, payments and access](accounts-payments-and-access.md) | Role matrices, payment states, consent rules, account isolation |
| [Compatibility and exploration](compatibility-and-exploration.md) | Environment sampling, keyboard checks, interruptions, exploratory charters |

## Common execution contract

Every run identifies the build, environment, date, fixture and account role. Record **Passed**, **Failed**, **Blocked** or **Not run** for each selected case; a design document alone has no execution status.

A pass requires the stated expectation to be checked. Separate a verified intermediate state from an unverified end-to-end result. For defects, retain reproduction steps and sanitized evidence; for blocked checks, name the missing condition and its release impact.

Use authorized test accounts and synthetic or approved media. Payment cases use a provider sandbox or an explicitly approved test mechanism. Never put credentials, payment details, private audio or personal transcripts into portfolio evidence.

[Profile](../README.md)

