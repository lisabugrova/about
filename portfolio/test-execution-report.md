# Audiodoc Studio — Recorded Upload Execution

**Artifact type:** historical execution record retained from the existing portfolio.  
**Scope:** two balance/upload scenarios. Expanded reference cases elsewhere are not execution results.

## Results and evidence

| Case | Conditions | Recorded actual behavior | Recorded outcome / evidence |
| --- | --- | --- | --- |
| TC-BAL-001 | Available time 0 s; supported 10 s video | File selected; Insufficient balance; message says 10 seconds more; normal Start upload action absent | Pass recorded — UI observation described |
| TC-BAL-002 | Available time 3600 s; supported 26:16 video | Ready state and Start upload action; successful processing, leave-page warning, download and editor access reported | Pass recorded — readiness described from screenshot; completion tester-reported |

For TC-BAL-001, the validation error is the expected business behavior, not a product defect. For TC-BAL-002, the intermediate ready state and the full processing result have different evidence sources.

## Original execution metadata

| Field | Available record |
| --- | --- |
| Application | Audiodoc Studio |
| Media | 10-second and 26:16 supported videos |
| Browser / OS | Not recorded in the supplied material |
| Build / release | Not recorded in the supplied material |
| Execution date | Not recorded in the supplied material |
| Account / dataset identity | Redacted in the original report |
| Screenshots | Described by the original report; attachments not included |

No build, environment, attachment or new pass result has been reconstructed from the source code.

## Follow-on execution

The original report identified 9/10/11-second eligibility, zero-balance drag-and-drop, combined file duration and rounding as next checks. These remain **not recorded as executed** here.

For a new run, capture build, environment, date, fixture identity, actual observations and sanitized evidence. Keep the old result and new result distinguishable; do not retroactively assign the new build to this record.

[Test design](audiodoc-studio-balance-validation.md) · [Case study](audiodoc-studio-case-study.md) · [Profile](../README.md)

