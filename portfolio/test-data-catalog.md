# Audiodoc Studio — Test Data Specification

**Purpose:** Test data for [file upload](audiodoc-studio-file-upload.md) and [balance validation](audiodoc-studio-balance-validation.md).

## Media fixtures

| Fixture | Format | Duration | File size | Used in |
| --- | --- | --- | --- | --- |
| `sample_video_10s.mp4` | Supported MP4 | **00:00:10** (10 s) | Approximately **2 MB**, comfortably below maximum | TC-BAL-001, TC-BAL-003, TC-BAL-004, TC-UPL-006 |
| `sample_video_26m16s.mp4` | Supported MP4 | **00:26:16** (1576 s) | Below **1700 MB** | TC-BAL-002, TC-UPL-001, TC-UPL-002 |
| `count_01.mp4` to `count_11.mp4` | Supported MP4 | **00:00:10 each** | Approximately **2 MB each** | TC-UPL-003 (first 10), TC-UPL-004 (all 11) |

Media files must be decodable, contain no personal or confidential information, and meet the application's supported-format requirements. Check the *actual* duration displayed by the application; a container's nominal duration can differ from the detected processing duration.

## Balance test matrix

| Data set | Selected file | File duration | Available time | Reason |
| --- | --- | --- | --- | --- |
| BAL-D01 | `sample_video_10s.mp4` | 10 s | **0 s** | Observed insufficient-balance condition |
| BAL-D02 | `sample_video_26m16s.mp4` | 1576 s | **3600 s** | Observed ready and reported successful processing |
| BAL-D03 | `sample_video_10s.mp4` | 10 s | **9 s** | One second below boundary |
| BAL-D04 | `sample_video_10s.mp4` | 10 s | **10 s** | Exactly at boundary |
| BAL-D05 | `sample_video_10s.mp4` | 10 s | **11 s** | One second above boundary |
| BAL-D06 | `sample_video_10s.mp4` | 10 s | **0 s → 3600 s** | Balance refresh while a file is selected |

**Environment:** test account with balances of 0, 9, 10, 11 and 3600 seconds.

## File-count test matrix

| Data set | Files to select | Count | Total video duration | Available time |
| --- | --- | ---: | --- | --- |
| COUNT-00 | Cancel the file picker without selecting a file | 0 | 0 s | 3600 s |
| COUNT-01 | `count_01.mp4` | 1 | 10 s | 3600 s |
| COUNT-09 | `count_01.mp4`–`count_09.mp4` | 9 | 90 s | 3600 s |
| COUNT-10 | `count_01.mp4`–`count_10.mp4` | 10 | 100 s | 3600 s |
| COUNT-11 | `count_01.mp4`–`count_11.mp4` | 11 | 110 s | 3600 s |

Reset the queue between each selection. Confirm whether the product's maximum of 10 applies per batch, queue or document inventory.

## File-size boundary data

**Interface limit:** 1700 MB per file. The table covers decimal MB and binary MiB interpretations.

| Test case | Decimal 1700 MB interpretation | Binary 1700 MiB interpretation |
| --- | ---: | ---: |
| 1 byte below | **1,699,999,999 B** | **1,782,579,199 B** |
| Exact limit | **1,700,000,000 B** | **1,782,579,200 B** |
| 1 byte above | **1,700,000,001 B** | **1,782,579,201 B** |

For the *decimal interpretation*, prepare valid **00:26:16** MP4 files with these names:

- `size_1699999999B.mp4` — 1,699,999,999 B
- `size_1700000000B.mp4` — 1,700,000,000 B
- `size_1700000001B.mp4` — 1,700,000,001 B

**Preconditions:** valid MP4 files of the specified sizes; balance **3600 s**; clear the queue between checks.

## Execution controls

Record the actual SHA-256/file length, parsed media duration, browser and OS, build, test date, available time before selection, and results in the execution log. Reset queue/balance as specified to avoid one test affecting the next.

[← Test design overview](test-design-workbook.md) · [← Portfolio](../README.md)
