# Elizaveta Bugrova

**QA Engineer · Manual Testing · Regression Testing · Release Verification**

Tbilisi, Georgia · [Telegram](https://t.me/lizavetamirr) · [GitHub](https://github.com/lisabugrova)

I am a QA engineer with commercial experience in a startup environment since 2024. I focus on checking user-facing functionality, identifying change-related risks, and making the results of testing clear enough to support release decisions.

My approach: **understand the expected behavior → assess risks → design purposeful checks → report evidence and remaining uncertainty.**

## Professional experience

### QA Engineer · Audiodoc · 2024–present

Areas of responsibility:

- **Functional and release verification:** final manual checks of changes and critical user journeys.
- **Regression testing:** selection and execution of checks based on change impact, affected workflows, and previously observed defects.
- **QA visibility:** tracking verification progress, documenting defects, and communicating blockers, coverage gaps, and open risks.
- **Knowledge transfer:** preparing another tester to take over recurring QA activities and understand the product's testing context.

*This is a high-level description of work areas, not a disclosure of internal company test data. Outcomes, tools, and metrics will be added only when verified.*

## Selected QA work samples

| Artifact | What it demonstrates | Evidence status |
| --- | --- | --- |
| **[Audiodoc Studio — Balance Validation](portfolio/audiodoc-studio-balance-validation.md)** | **Observed negative and positive cases:** insufficient-balance error for a 10-second video at 0 balance; a 26:16 video ready to process at 1 hour, followed by tester-reported successful output | Screenshots for validation/ready states; completion **tester-reported**; no versioned test-run log |
| [Audiodoc Studio — File Upload & Available-Time Gate](portfolio/audiodoc-studio-file-upload.md) | Upload limits, EP/BVA, decision table, sample checks and evidence-aware reporting | Mixed: observed balance cases; other tests **Not Run** |
| [Test design workbook — Audiodoc Studio](portfolio/test-design-workbook.md) | Choosing a fitting design technique for each real interface area and documenting unanswered requirements | Design / analysis |
| [Regression & release assessment](portfolio/regression-and-release.md) | Change-impact reasoning, risk-based regression and release summaries | Illustrative process sample |
| [QA handover playbook](portfolio/qa-handover-playbook.md) | Transfer of product knowledge, test assets, routines and independent ownership criteria | Illustrative process sample |

### Additional learning exercises

- [Pairwise drag-and-drop case](portfolio/pairwise-drag-drop.md) — **fictional task parameters**, selected combinations; *not an executed Audiodoc test*.
- [Sample defect reports](portfolio/defect-report-samples.md) — **fictional defects**, designed to demonstrate reporting structure; *not real production findings*.

**Evidence policy:** Audiodoc Studio work samples distinguish **what screenshots show**, **what a tester reports**, and **what remains untested**. The balance restriction behaves as expected in the observed zero-balance case; any request for an explicit top-up action is an **unconfirmed UX acceptance criterion**, not a confirmed defect. Other learning examples remain explicitly fictional. Uploaded screenshots, file names, client files and confidential records are not republished.

## How I think about quality

- Start with requirements, user outcomes, constraints, and failure impact—not just a long checklist.
- Select test techniques to fit the risk: partitions and boundaries for inputs, decision tables for rules, state transitions for interactions.
- Distinguish **test coverage** from the number of test cases and **test execution status** from product quality.
- When reporting readiness, state what passed, what failed, what was blocked, and what was *not tested*.
- Keep test materials understandable for developers, product stakeholders, and the next QA engineer.

---

**Looking to grow in a larger engineering organization**, contribute to a mature quality process, and build on hands-on release and regression testing experience.
