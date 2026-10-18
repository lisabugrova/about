# QA handover

Reference plan for handing QA work to another engineer.

## Work through a change

| Stage | Work | Ready when |
| --- | --- | --- |
| Product walkthrough | Follow the core journey; explain roles, processing time, editor variants and sharing. | The incoming engineer can explain the main risks using the [coverage map](product-coverage.md). |
| Setup | Prepare the target build, test accounts, synthetic files, payment sandbox and required services. | Setup is repeatable; missing prerequisites are listed; documentation contains no credentials. |
| Shared testing | Run a happy path, a negative case and a save/recovery case. | Results name the build, environment and data, separating observations from reported or unverified behavior. |
| Defect follow-up | Reproduce a real issue within the agreed scope, discuss its impact with a developer and retest a fix if available. | The report is reproducible and the retest names the build. Use an existing issue if no new defect is found. |
| Independent testing | The incoming engineer selects and runs checks for a real change; the outgoing engineer reviews the results. | The release recommendation explains exclusions and remaining risks; review comments are addressed. |

## Hand over

- [Coverage and test data](product-coverage.md), [test cases](test-design-workbook.md) and [regression checks](regression-and-release.md).
- Execution records with build, environment, data and evidence; keep old and new results distinct.
- The team's defect tracker: open issues, resolved issues, reproduction steps and retest status.
- Internal contacts for requirements, deployment, test accounts, payment sandbox and release decisions.

## Audiodoc details to explain

- The displayed “1700 MB” uses a binary byte limit; check the target build before changing the expected threshold.
- Unknown duration may require transfer and a server probe before the final processing check.
- Shared links require sign-in. Reading, editing and ownership have different permissions.
- Classic and beta word-based editors differ; name the editor being tested.
- Check saved content, exported files and balance separately from the UI's success message.

Finish when the incoming engineer can prepare and run checks, report findings, follow fixes and explain what remains untested. Review the next cycle to check that the handover holds up in practice.

[Profile](../README.md) · [Design index](test-design-workbook.md)
