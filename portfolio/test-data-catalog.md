# Audiodoc Studio — Test Data Specification

**Purpose:** Reproducible data for the [file-upload](audiodoc-studio-file-upload.md) and [balance-validation](audiodoc-studio-balance-validation.md) test cases. The names below identify **files to prepare**; they are not links to already uploaded media.

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
| BAL-D03 | `sample_video_10s.mp4` | 10 s | **9 s** | Planned: 1 second below boundary |
| BAL-D04 | `sample_video_10s.mp4` | 10 s | **10 s** | Planned: exactly at boundary |
| BAL-D05 | `sample_video_10s.mp4` | 10 s | **11 s** | Planned: 1 second above boundary |
| BAL-D06 | `sample_video_10s.mp4` | 10 s | **0 s → 3600 s** | Planned: balance refresh while a file is selected |

Prepare balances in an approved test account using a permitted test-data setup; do not use real purchases solely to establish these test conditions without authorization. **BAL-D03–05 are proposed execution data; the exact acceptance rule and rounding have not been verified.**

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

The displayed maximum is **1700 MB per file**. Exact byte limits must be confirmed before execution.

| Test case | Decimal 1700 MB interpretation | Binary 1700 MiB interpretation |
| --- | ---: | ---: |
| 1 byte below | **1,699,999,999 B** | **1,782,579,199 B** |
| Exact limit | **1,700,000,000 B** | **1,782,579,200 B** |
| 1 byte above | **1,700,000,001 B** | **1,782,579,201 B** |

For the *decimal interpretation*, prepare valid **00:26:16** MP4 files with these names:

- `size_1699999999B.mp4` — 1,699,999,999 B
- `size_1700000000B.mp4` — 1,700,000,000 B
- `size_1700000001B.mp4` — 1,700,000,001 B

With a binary interpretation, produce valid media at the alternate byte sizes and name each file by its actual byte length. Use **3600 s** available time for each individual file-size check. Do not simply corrupt or append arbitrary bytes to a media container: the fixture must remain a valid, supported video. Large-file checks require approved storage, bandwidth and test environment capacity.

## Execution controls

Record the actual SHA-256/file length, parsed media duration, browser and OS, build, test date, available time before selection, and results in the execution log. Reset queue/balance as specified to avoid one test affecting the next.

[← Test design overview](test-design-workbook.md) · [← Portfolio](../README.md)
