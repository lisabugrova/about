# Audiodoc Studio — Upload Validation Test Execution Summary

**Feature:** Your documents → Upload files → Processing  
**Related design:** [Balance Validation & Processing](audiodoc-studio-balance-validation.md)  
**Test data:** supported videos of **10 seconds** and **26 minutes 16 seconds**  
**Execution evidence:** interface screenshots for file validation and readiness; processing/completion behavior confirmed by the tester's account.

## Results

| Test case | Preconditions | Expected result | Actual result | Result |
| --- | --- | --- | --- | --- |
| [TC-BAL-001](audiodoc-studio-balance-validation.md#tc-bal-001--insufficient-balance-prevents-upload) | Balance **0 s**, video **10 s** | Prevent upload; show insufficient balance and **10 s** shortfall | Selected file displays **Insufficient balance** and **Not enough time, you need 10 seconds more**; **Start upload!** absent | **Pass** — UI observed |
| [TC-BAL-002](audiodoc-studio-balance-validation.md#tc-bal-002--process-a-video-with-sufficient-balance) | Balance **1 hour**, video **26:16** | Ready state; processing; download and editor | Screenshot shows **Ready to upload!** and **Start upload!**. Tester reports successful upload and processing, warning not to leave the page, and working download/editor actions | **Pass** — ready state observed; completion tester-reported |

## Test execution record

### TC-BAL-001 — Insufficient balance

**Actual behavior**
1. The **10-second** video appears in the selected-file list despite **0 seconds** remaining.
2. The file status is **Insufficient balance**.
3. The shortage message states **10 seconds more**.
4. The normal **Start upload!** action is not shown.

**Assessment:** The insufficient-balance validation works as expected for this scenario. An expected validation error is not itself a product defect.

### TC-BAL-002 — Sufficient balance

**Actual behavior**
1. A **26:16** video is selected with **1 hour** available.
2. The file state is **Ready to upload!**; **Start upload!** is available.
3. After the action, the tester reports that upload and processing complete successfully and a leave-page warning is shown while processing.
4. The processed result can be downloaded or opened in the editor.

**Assessment:** The observed ready state and the tester-reported completion match the expected workflow.

## Execution metadata

| Field | Value |
| --- | --- |
| Application | Audiodoc Studio |
| Browser / OS | To record |
| Build / release | To record |
| Execution date | To record |
| Test account / dataset | Redacted |
| Screenshot attachments | Not included in public report |

## Recommended next execution

- Using a supported **10-second** video, check balances **9 s, 10 s and 11 s** (not yet executed).
- Repeat zero-balance validation via drag-and-drop.
- Verify shortage calculation for multi-file selection and rounding.
- Record build, browser, date and sanitized evidence for repeatability.

[← Portfolio](../README.md)
