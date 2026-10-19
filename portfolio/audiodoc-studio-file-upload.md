# Elizaveta Bugrova

[Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

Language: [Русский](../ru/portfolio/audiodoc-studio-file-upload.md) · [English](audiodoc-studio-file-upload.md)

## Test cases: upload and balance

**Task:** check file-selection rules and whether processing can start. This is test design; the table does not record execution.

**Common setup:** signed-in account, empty queue and valid MP4 files. For count checks, use distinct 10-second files and a balance of 3600 seconds. For size checks, the balance must cover each file; duration is not fixed.

### Selected cases

| Case | Data and action | Expected result |
| --- | --- | --- |
| TC-UPL-003 · At and below the count limit | Select 9 files, clear the queue, then select 10. | Nine files are accepted in the first attempt; ten in the second. |
| TC-UPL-004 · Above the limit | Select 11 files. | No more than 10 are accepted; a count-limit message appears. |
| TC-UPL-005 · File-size boundary | Confirm the size limit **L** in bytes. Select valid files of **L−1**, **L**, **L+1** bytes separately, clearing the queue each time. | L−1 and L satisfy the size rule; L+1 is rejected. Confirm the MB/MiB unit before choosing fixtures. |
| TC-UPL-006 · Clear selection | Select a valid file, clear the queue, then select the same file again. | After clearing, the queue is empty. Reselection adds exactly one entry with the filename, duration and validation status. |
| TC-BAL-001 · Zero balance | Set balance to 0 seconds; select a 10-second video. | File can be selected; processing cannot start. |
| TC-BAL-003 · Balance boundary | Upload videos of 29, 30 and 31 minutes with a balance of 30 minutes. Clear the queue between attempts. | Under minute-level accounting: processing is allowed for 29 and 30 minutes, but blocked for 31. |
