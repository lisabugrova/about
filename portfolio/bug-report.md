# Elizaveta Bugrova

[Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

## Bug report

**Example prepared for this portfolio. It is not a defect found in Audiodoc.**

| Field | Example |
| --- | --- |
| ID | DEMO-UPL-01 |
| Summary | **Document upload window: adding a second batch allows the ten-file limit to be exceeded.** |
| Area | File upload → selected-file queue |
| Environment | macOS 26.6.2, Safari Version 26.6.2. |
| Preconditions | Signed-in account; empty queue; 3600 seconds available; 11 distinct, valid MP4 files. |
| Step 1 | Select the first 6 files. |
| Step 2 | Open the file picker again and select the other 5 files. |
| Step 3 | Count the files in the queue and inspect validation. |
| Expected result | An attempt to exceed the limit produces a limit message. |
| Actual result | The queue contains all 11 files and shows no count-limit message. |
| Impact | **The queue limit can be bypassed by adding files in separate batches.** |
| Suggested severity | Medium: validation is bypassed. |
| Priority to fix | Set by the PM |
