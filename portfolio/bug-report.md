# Elizaveta Bugrova

[Русский](../ru/portfolio/bug-report.md) · **English** · [Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

## Bug report: filled example

**Hypothetical example prepared for this portfolio. It is not a defect found in Audiodoc.**

| Field | Example |
| --- | --- |
| ID | DEMO-UPL-01 |
| Summary | Adding a second batch allows the queue to exceed the ten-file limit. |
| Area | File upload → selected-file queue |
| Environment | Illustrative desktop web scenario; no application run was performed. |
| Preconditions | Signed-in account; empty queue; 3600 seconds available; 11 distinct, valid 10-second MP4 files. |
| Step 1 | Select the first 6 files. |
| Step 2 | Open the file picker again and select the other 5 files. |
| Step 3 | Count the files in the queue and inspect validation. |
| Expected result | The queue contains at most 10 files. An attempt to exceed the limit produces a clear message. |
| Actual result in this hypothetical scenario | The queue contains all 11 files and shows no count-limit message. |
| Impact | The cumulative queue limit can be bypassed by adding files in separate batches. |
| Suggested severity | Medium: validation is bypassed; data loss is not established. |
| Priority to fix | To be set during triage, considering release impact, affected users and an available workaround. |
| Evidence for a real report | A recording of both additions, final queue and validation state; observation date, exact browser/build and repeatability. |

**Reporting choice:** the title names the failure and its trigger. The report separates visible behavior from impact; it does not assume that all 11 files were processed or charged.
