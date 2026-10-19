# Elizaveta Bugrova

[Русский](../ru/portfolio/audiodoc-studio-file-upload.md) · **English** · [Telegram](https://t.me/lizavetamirr) · [Résumé](../README.md)

## Test cases: upload and balance

**Task:** check file-selection rules and whether processing can start. This is test design; the table does not record execution.

**Common setup:** signed-in account, empty queue and valid MP4 files. For count checks, use distinct 10-second files and a balance of 3600 seconds. For size checks, the balance must cover each file; duration is not fixed.

### Selected cases

| Case | Data and action | Expected result |
| --- | --- | --- |
| TC-UPL-003 · File-count limit | Select 10 files. | All 10 can be selected. |
| TC-UPL-004 · Above the limit | Select 11 files. | No more than 10 are accepted; a count-limit message appears. |
| TC-UPL-005 · File-size boundary | Confirm the size limit **L** in bytes. Select valid files of **L−1**, **L**, **L+1** bytes separately, clearing the queue each time. | L−1 and L satisfy the size rule; L+1 is rejected. Confirm the MB/MiB unit before choosing fixtures. |
| TC-UPL-006 · Clear selection | Select files, clear the queue, then select again. | The old selection is removed; the new selection works normally. |
| TC-BAL-001 · Zero balance | Set balance to 0 seconds; select a 10-second video. | File can be selected; a 10-second shortage is shown; processing cannot start. |
| TC-BAL-003 · Balance boundary | Select the same 10-second video with balances of 9, 10 and 11 seconds, resetting the queue between attempts. | Under second-level accounting: 9 seconds blocks processing; 10 and 11 allow it. |

### Why these checks

| Decision | Reason |
| --- | --- |
| Enough balance for every count/size fixture | One validation rule should not mask another. |
| Values immediately below, at and above a limit | Check both the boundary and whether equality is accepted. |
| Separate selection from permission to process | A file may remain selectable while processing is blocked. |
