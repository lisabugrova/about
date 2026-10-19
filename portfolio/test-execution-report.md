# Elizaveta Bugrova

[Русский](../ru/portfolio/test-execution-report.md) · **English** · [Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

## Test results

**Selected records from the existing portfolio.** Statuses below are retained from that report.

| Scenario | Expected | Recorded behavior | Status |
| --- | --- | --- | --- |
| TC-BAL-001 · 0 seconds available; 10-second video | Selection allowed; insufficient-balance message; processing blocked. | File selected; 10-second shortage shown; normal start action absent. | Passed — UI observation described. |
| TC-BAL-002 · 3600 seconds available; video 26:16 | Ready state; processing; download and editor access. | Readiness described from a screenshot. The tester reported successful processing, a leave-page warning and working result actions. | Passed — readiness described; completion tester-reported. |

**Record details:** browser/OS, build and date were not included. Screenshots were described but are not attached.

### Follow-up checks

These were listed as not yet executed.

| Check | Purpose |
| --- | --- |
| 10-second video with balances of 9, 10 and 11 seconds | Verify the eligibility boundary. |
| Zero-balance selection through drag-and-drop | Check the alternate input path. |
| Multiple files and duration rounding | Check the combined time requirement. |
