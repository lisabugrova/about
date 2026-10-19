# Audiodoc Studio — manual QA

[CV](../CV.md) · [Русский](../ru/portfolio/audiodoc-studio-case-study.md)

## Project and selected work

Audiodoc Studio turns audio and video into transcripts that users can edit alongside the recording. Processing is paid for with a time balance.

I worked on the product as a manual QA engineer. The samples here focus on upload and processing-balance validation: requirements, test data, expected behavior, regression selection and recorded results.

## The testing problem

Several rules apply to the same upload. A supported file may fit the size limit but exceed the account's processing balance. A group of valid files may exceed the queue limit. If the test data triggers several restrictions, it can be difficult to tell which rule caused the result.

The test design separates those conditions and links checks to the relevant requirements.

## Design choices

**Isolate the rule under test.** To check the ten-file queue limit, the sample uses eleven short files and enough balance to process all of them. A balance error should not mask a count-limit failure.

**Make boundaries explicit.** The size sample checks one byte below, exactly at and one byte above its stated limit. File duration is independent of this check; the balance must cover each file. The sample records its decimal-MB assumption so the unit can be confirmed against requirements before execution.

**Cover business states.** The balance design uses a decision table for insufficient and sufficient time, including the equality boundary. State transitions connect selection, readiness, processing and access to the result.

**Choose regression checks by impact.** The regression document links changes in the balance rule, queue and processing flow to affected checks. It also includes coverage gaps and remaining risk in the release-summary template.

## Work samples

- [Upload test design](audiodoc-studio-file-upload.md): requirements, fixtures, boundaries and traceability.
- [Balance and processing test design](audiodoc-studio-balance-validation.md): decision table, test cases and state transitions.
- [Regression selection](regression-and-release.md): affected areas, priorities and release-report structure.
- [Execution record](test-execution-report.md): selected results for insufficient and sufficient balance.

## Recorded results

The execution record describes selection of a 10-second video at zero balance, followed by the expected shortage message and blocked processing action. It also records a 26:16 video with one hour available: a ready state, followed by reported processing completion, download and editor access.

The report distinguishes the described UI observations from reported completion. Available evidence and follow-up checks are documented with the results.

[All work samples](test-design-workbook.md)
