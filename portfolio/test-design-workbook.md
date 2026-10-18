# QA portfolio

[CV](../CV.md) · [Audiodoc case study](audiodoc-studio-case-study.md)

## Materials

| Material | Contents |
| --- | --- |
| [Coverage map](product-coverage.md) | Product areas, risks and test data |
| [Upload tests](audiodoc-studio-file-upload.md) | Selection, queue limits, file size and media duration |
| [Balance tests](audiodoc-studio-balance-validation.md) | Eligibility, rounding, combined duration, retry and accounting |
| [Editor and exports](editor-and-output.md) | Text, audio alignment, speakers, stories, saving and output files |
| [Accounts, payments and access](accounts-payments-and-access.md) | Authentication, consent, purchases, sharing, projects and referrals |
| [Compatibility and exploration](compatibility-and-exploration.md) | Browsers, devices, keyboard access, languages and interruptions |
| [Execution record](test-execution-report.md) | Two scenarios retained from the original portfolio |
| [Regression and release](regression-and-release.md) | Selecting checks and a worked release report |
| [Defect report](defect-report-template.md) | A worked reporting example |
| [QA handover](qa-handover-playbook.md) | Setup, shared testing and transfer of responsibility |

## About these materials

The upload/balance samples and two execution records came from Elizaveta's original portfolio. The expanded designs were prepared with the project owner and AI assistance after reviewing the application source on **18 October 2026**. They are reference material, not independently authored historical work or completed test runs.

The defect and release examples are fictional and labeled accordingly. The process documents describe how to organize the work.

The suite covers the reviewed application's main user-facing areas. It does not establish coverage of every input or the separate transcription service. A deployed build may differ from the reviewed source.

## Running the checks

Record the build, browser/device, date, fixture and account role. Use **Passed**, **Failed**, **Blocked** or **Not run**. A pass requires checking the expected result; reaching an intermediate screen does not establish end-to-end success.

Attach reproduction steps and sanitized evidence to defects. For a blocked check, name the missing prerequisite and its effect on the release decision.

Use test accounts, synthetic or approved media, and a payment sandbox. Keep credentials, payment details and customer recordings out of the portfolio.

[Profile](../README.md)
