# QA Handover — Knowledge Transfer & Readiness Checklist

**Objective:** Transfer recurring QA work so another engineer can independently select checks, execute them, report defects and communicate remaining risks.

## Handover matrix

| Topic | Materials to transfer | Independence check |
| --- | --- | --- |
| Product workflows | User journeys: select → validate → upload → process → retrieve result | Explain key workflows and failure paths without prompts |
| Requirements | Balance and file-limit rules; questions requiring confirmation | Identify test basis and clarify ambiguous rules |
| Test design | [File upload](audiodoc-studio-file-upload.md), [balance validation](audiodoc-studio-balance-validation.md) | Adapt existing test cases to a changed requirement |
| Test execution | Test environment, test data, evidence requirements, [execution summary](test-execution-report.md) | Complete a small test run and record results |
| Defect reporting | [Defect report template](defect-report-template.md) | Produce a reproducible report with evidence |
| Regression | [Risk-based regression selection](regression-and-release.md) | Justify which scenarios are required for a given change |
| Release communication | Scope, passed/failed/blocked, open issues, residual risk | Prepare a reviewable release test summary |

## Transfer sequence

1. **Product walkthrough:** map critical journeys and stakeholders.
2. **Requirements review:** distinguish documented rules from open questions.
3. **Guided test run:** execute a balance-validation case together.
4. **Independent test design:** prepare cases for a small change and review the coverage.
5. **Defect-report exercise:** write reproduction steps and expected/actual results.
6. **Regression selection:** select checks according to impact and risk.
7. **Readiness review:** ask the trainee to run the checks, report status and explain remaining risk.

## Readiness checklist

- [ ] Can set up a test environment and use safe test data.
- [ ] Can explain the upload and balance-validation workflow.
- [ ] Can select and adapt EP, BVA, decision-table and state-transition checks.
- [ ] Can reproduce a failure and write an actionable defect report.
- [ ] Can distinguish **passed, failed, blocked and not run**.
- [ ] Can select an appropriate regression subset for a change.
- [ ] Can prepare a release test summary and escalate unresolved risks.
- [ ] Knows ownership, escalation contacts and documentation locations.

[← Portfolio](../README.md)
