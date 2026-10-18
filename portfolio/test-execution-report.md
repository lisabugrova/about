# Audiodoc Studio: upload test results

Two scenarios retained from the original portfolio.

| Case | Conditions | Recorded behavior | Result and evidence |
| --- | --- | --- | --- |
| TC-BAL-001 | 0 s available; 10 s video | File selected; Insufficient balance; 10-second shortage; normal Start upload action absent | Passed in the original report; UI observation described |
| TC-BAL-002 | 3600 s available; 26:16 video | Ready state and Start upload action; processing, leave-page warning, download and editor access reported | Passed in the original report; readiness described from a screenshot, completion tester-reported |

The first case checks an expected restriction. An insufficient-balance message is the correct result.

## Available evidence

Browser/OS, build and execution date were not recorded. Account and dataset identifiers were redacted. Screenshots were described but not attached.

The report therefore supports different levels of detail: UI behavior is described for both cases; the full processing outcome in TC-BAL-002 is the tester's account.

## Next checks

The original report listed these as still to be run:

- 10-second video at 9/10/11-second balances.
- Zero-balance validation through drag-and-drop.
- Combined file duration and rounding.

New runs should have their own build, environment, date and evidence.

[Balance design](audiodoc-studio-balance-validation.md) · [Case study](audiodoc-studio-case-study.md) · [Profile](../README.md)
