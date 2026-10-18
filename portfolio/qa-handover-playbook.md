# QA Handover — Operational Reference

**Artifact type:** process reference. This is a usable handover plan, not a claim that a particular transfer has already been completed.

## Objective

Enable the incoming QA engineer to take a change from requirements through execution, defect follow-up and release reporting, using the same product understanding and evidence standards.

## Handover sequence

| Stage | Work together | Deliverable / exit criterion |
| --- | --- | --- |
| 1. Product orientation | Walk the core journey and the failure consequences. Explain user roles, processing time, editor variants and sharing. | Incoming engineer can describe the journey and the highest risks using the [coverage map](product-coverage.md). |
| 2. Environment and data | Identify supported builds, test accounts, synthetic fixtures, payment sandbox and service dependencies. | A repeatable setup; no credentials in documentation; known blocked prerequisites listed. |
| 3. Shared execution | Execute one happy path, one negative path and one persistence/recovery path. | Results identify build/environment/data and separate observed, reported and unverified behavior. |
| 4. Defect workflow | Reproduce an actual authorized issue, agree impact, discuss with a developer, and retest a fix if available. | Another person can reproduce the report; retest links to the changed build. Do not invent a defect to satisfy the exercise. |
| 5. Independent cycle | Incoming engineer selects scope for a real change, executes and reports; outgoing engineer reviews reasoning. | A coherent release recommendation with exclusions and residual risks; corrections incorporated. |

## Materials to transfer

- [Coverage and fixtures](product-coverage.md): critical journeys, rule boundaries, product-specific expectations.
- [Feature designs](test-design-workbook.md): reusable cases and their intended scope.
- Execution records: actual build, environment, data and evidence; historical results stay identifiable.
- Defect register: open/resolved items, impact, reproduction and retest status in the team's tracker.
- [Regression and release](regression-and-release.md): prioritization and decision format.
- Operational contacts: owners of requirements, deployments, accounts, provider sandbox and release decisions, recorded in the internal team workspace.

## Product-specific knowledge

- “1700 MB” in the UI uses a binary byte limit; do not change the expected threshold without checking the target build.
- Unknown media duration may require transfer/probe before the final processing check.
- Shared links still require authentication; reading, editing and ownership are separate permissions.
- Classic and beta word-based editors have different capabilities; the current test target must be named.
- Successful UI feedback does not by itself prove saved content, usable export or reconciled balance.

## Completion

The incoming engineer can independently choose a useful scope, prepare data, execute, report reproducible findings, follow a fix and explain what remains untested. Review one subsequent cycle to confirm the process remains usable.

[Profile](../README.md) · [Design index](test-design-workbook.md)

