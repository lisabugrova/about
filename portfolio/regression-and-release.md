# Elizaveta Bugrova

[Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

## Regression and QA handover

**Process example:** selecting checks after a change to upload or balance validation, reporting results and transferring recurring work.

### Choosing regression checks

| Changed area | What to check | Why |
| --- | --- | --- |
| Balance validation | Below, equal to and above the required time; zero balance. | Avoid allowing unpaid processing or blocking an eligible file. |
| File selection and queue | Picker/drop, queue limit, removal and reselection. | Find differences between input paths and stale validation. |
| Upload/processing | Start, completion, download and editor access. | Check that the main user journey still works. |
| Balance refresh | Recheck the selected file after the balance changes. | Verify that eligibility is recalculated. |

### Reporting and handover

| Stage | What to prepare | Completion check |
| --- | --- | --- |
| Test report | Build/environment, selected checks, results, defects, blocked and untested areas. | The team can see what was checked and what risk remains. |
| Fix verification | Repeat the previously failing steps on the fixed build; add checks for the fix where needed. | The original failure no longer occurs. Related behavior is checked separately by regression testing. |
| QA handover | Product rules, fixtures, test cases, recurring checks and known issues. | Another tester can prepare and run the checks, record results and identify blockers. |

**Selection rule:** prioritize the changed behavior and its dependencies. Record exclusions so a short regression run does not imply full product coverage.
