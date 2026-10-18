# Audiodoc Studio: upload and balance testing

Audiodoc Studio transcribes audio and video. Users can check the transcript against the recording, edit it, assemble stories and export the result. Processing is paid for with a time balance.

## The testing problem

File validation and balance validation overlap. A file can be supported and within the size limit but still be too long for the available balance. A test needs to isolate the rule it is checking.

The original work samples cover selection through the file picker and drag-and-drop, queue limits, file size, duration, balance, processing readiness and clearing the selection.

## Three examples

**File count.** Prepare eleven short files and give the account enough processing time for all of them. Check nine, ten and eleven files. This keeps the balance gate from obscuring the ten-file limit.

**File size.** Check immediately below, at and above the limit. The original design identified uncertainty about MB versus MiB. Source review for the expanded reference resolved the current threshold: 1,782,579,200 bytes, inclusive.

**Balance.** A 10-second video at zero balance should remain selectable but cannot proceed to normal processing. The expected insufficient-balance message is a successful validation result. The 9/10/11-second balance checks test the eligibility boundary.

[Upload design](audiodoc-studio-file-upload.md) · [Balance design](audiodoc-studio-balance-validation.md)

## Recorded results

The original report describes the zero-balance case above and a 26:16 video with one hour available. In the second case, the ready state was described from a screenshot; successful processing, download and editor access were reported by the tester.

Build, browser, date and screenshot attachments were not included. The [execution record](test-execution-report.md) retains that distinction.

The wider [reference suite](test-design-workbook.md) adds editing, access, payments and other product areas. Its authorship and scope are explained there.

[Profile](../README.md)
