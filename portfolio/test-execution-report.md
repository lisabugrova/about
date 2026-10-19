# Audiodoc Studio — selected test results

[Case study](audiodoc-studio-case-study.md) · [Русский](../ru/portfolio/test-execution-report.md)

These two scenarios are selected records from the portfolio. They show how expected behavior and actual results were documented for upload validation and processing.

| Case | Conditions | Recorded result | Evidence recorded |
| --- | --- | --- | --- |
| [TC-BAL-001](audiodoc-studio-balance-validation.md#tc-bal-001--insufficient-balance-prevents-upload) | 0 s available; 10 s video | File selected; insufficient balance; 10-second shortage; normal processing action absent. Recorded as passed. | UI observation described |
| [TC-BAL-002](audiodoc-studio-balance-validation.md#tc-bal-002--process-a-video-with-sufficient-balance) | 3600 s available; 26:16 video | Ready state and start action; processing, leave-page warning, download and editor access reported. Recorded as passed. | Readiness described from a screenshot; completion reported in the test record |

For the first scenario, the insufficient-balance message is the expected validation result.

## Record details

Browser/OS, build and execution date were not included. Account and dataset identifiers were redacted. Screenshots were described but are not attached. The report is retained with those limitations.

## Follow-up checks listed in the report

- A 10-second video at 9/10/11-second balances.
- Zero-balance validation through drag-and-drop.
- Combined file duration and rounding.

These follow-up checks were not recorded as executed. Further runs should include their own environment, date and evidence.

[All work samples](test-design-workbook.md)
