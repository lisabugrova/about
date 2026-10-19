# Expanded product QA reference

Optional reference material for Audiodoc Studio, prepared with product-owner input and AI-assisted source review. It is separate from [Elizaveta's selected samples](../portfolio/test-design-workbook.md) and does not claim she independently authored or executed this expanded suite.

The five scenario documents contain **94 proposed checks**, supported by a coverage map. No completed runs are recorded here.

| Document | Contents |
|---|---|
| [Product coverage](product-coverage.md) | Main workflows, risks and test data |
| [Upload and media validation](audiodoc-studio-file-upload.md) | File selection, queue limits, metadata and media validation |
| [Balance and processing](audiodoc-studio-balance-validation.md) | Eligibility, batch processing, recovery and time accounting |
| [Editor and output](editor-and-output.md) | Text/audio alignment, speakers, stories, saving and exports |
| [Accounts, payments and access](accounts-payments-and-access.md) | Authentication, consent, payments, referrals, projects and sharing |
| [Compatibility and exploration](compatibility-and-exploration.md) | Browsers, devices, accessibility samples and exploratory sessions |

## Basis and limits

- **Review date:** 18 October 2026.
- **Application source:** [`istima/audiodoc-studio@164fa3cbaac16d419564970a2113d3bcfa90ca78`](https://github.com/istima/audiodoc-studio/tree/164fa3cbaac16d419564970a2113d3bcfa90ca78) — private repository; access required.
- **Original sample snapshot:** `lisabugrova/about@9e0aa24ff3d021b889e0d7c900897e6df48b36d3`. Those samples remain under `portfolio/` with their original scope.

Source code describes implementation at that commit; it does not establish agreed requirements or prove the deployed services behave the same way. Resolve open rules with the product owner before execution. This review did not validate a deployment.

The original upload sample assumes **1700 decimal MB (1,700,000,000 bytes)**. This reference uses the inspected implementation's **1700 MiB (1,782,579,200 bytes)**. Keep the two test bases distinct until the unit requirement is agreed.

[Profile](../README.md) · [Selected samples](../portfolio/test-design-workbook.md) · [Recorded execution](../portfolio/test-execution-report.md)
