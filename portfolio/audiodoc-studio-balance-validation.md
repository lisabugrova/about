# Audiodoc Studio — Insufficient Balance vs Successful Upload

**QA work sample:** manual functional test cases with actual observations, using the *Your documents* interface.
**Techniques:** Equivalence Partitioning (EP), Decision Table Testing; State Transition Testing for the observed successful journey; Boundary Value Analysis (BVA) suggested for further work.
**Evidence:** two screenshots supplied during test analysis, plus tester's description of the processing/completion flow. Screenshots are **not republished** here; file names are deliberately anonymized.
**Test environment:** product UI shown; exact build, browser/OS versions, account identifier and test dates not recorded. Treat results as **observed / tester-reported**, not a reproducible, environment-complete test execution record.

## 1. Test basis and expected behavior

- **BR-BAL-001 — Insufficient balance:** If the available processing time does not cover the selected video's duration, the user can select the file, but the upload/processing action is not available; an **insufficient balance** error should inform the user of the time shortage.
- **UX-BAL-002 — Top-up guidance (proposed acceptance criterion, unconfirmed):** The error should explicitly tell the user to purchase/add sufficient time or provide a clear action to do so. This wording/CTA has **not** been confirmed as an approved product requirement.
- **BR-BAL-003 — Sufficient balance (scenario-specific observation):** With 1 hour available and a 26 min 16 sec video, the file reaches the **Ready to upload!** state and **Start upload!** is shown. Upon clicking Start upload!, the tester reports that processing begins, a notice asks the user not to leave the page, and after successful processing a result can be downloaded or opened in the editor. These observations do **not** establish the exact rules for all positive balances.

**Terminology:** Here “error” means the application's **expected validation/error state**, not necessarily a software defect. A failure is a divergence between expected and actual behavior. Actual screen text says **“Not enough time”**, not “video is too short.”

## 2. Negative test case: insufficient available time

### TC-BAL-001 — Show insufficient balance error and block upload

| Field | Value |
| --- | --- |
| Test type | Negative functional test; business-rule validation |
| Technique | EP; Decision Table Testing |
| Priority | High |
| Preconditions | Signed-in user; available time **0 seconds**; supported test video of **10 seconds**; *Your documents* page is available |
| Test data | Video A, duration **00:00:10** (anonymized) |
| Expected | File selection is possible, but upload/processing cannot start when the balance is insufficient. The UI communicates **Insufficient balance** and the missing duration (**10 seconds**). Ideally, it provides explicit instructions or a CTA to purchase/add processing time (UX-BAL-002, **not yet confirmed**). No successful upload is started. |
| Actual — screenshot | Selected video appears in the list with **“Insufficient balance”**, **“10 seconds”** and **“Not enough time, you need 10 seconds more”**. No **Start upload!** action is visible in this state; the **Purchase time** panel is available elsewhere on the page. |
| Outcome | **Observed: core balance guard behaves as expected.** Explicit top-up instruction within the error is **not visible**; whether this is a defect requires an agreed UX requirement. |

**Steps to reproduce**
1. Open **Your documents** and confirm **Available time = 0 seconds**.
2. Select a supported, **10-second** video through the visible file-upload area.
3. Inspect the file row for validation messages and duration.
4. Verify whether **Start upload!** is available.
5. Record the shortage and compare it with the file duration (10 − 0 = **10 seconds**).

**Evidence source:** screenshot 1 supplied by tester. Exact error wording and balance/duration displayed in the screenshot. Upload blocking is inferred from absence of **Start upload!** and the tester's statement, not from a captured network trace.

### UX-FINDING-001 — Missing explicit top-up instruction in the error (candidate, not confirmed defect)

- **Observation:** The file-row error identifies insufficient time and the shortage, but does not visibly say “Purchase more time” or provide an inline purchase action. A **Purchase time** widget is shown elsewhere on the page.
- **Why it may matter:** the user may need to work out independently where to resolve the error.
- **Expected if UX-BAL-002 is accepted:** an actionable message such as *“Insufficient balance. You need 10 more seconds. Purchase additional time to continue.”*
- **Classification:** **UX improvement / potential defect — requires confirmation**. Do not file as a confirmed bug or assign a severity without checking the agreed requirements and design.
- **Suggested verification:** inspect design, business requirements, accessibility and product decision on whether the existing purchase widget is sufficient guidance.

## 3. Positive test case: enough available time

### TC-BAL-002 — Upload and process a video with sufficient balance

| Field | Value |
| --- | --- |
| Test type | Positive end-to-end functional scenario |
| Technique | EP (sufficient-balance class); observed workflow / states |
| Priority | High |
| Preconditions | Signed-in user; available time **1 hour**; supported video duration **26 minutes 16 seconds** |
| Test data | Video B, duration **00:26:16** (anonymized) |
| Expected | File is accepted as **Ready to upload!**; **Start upload!** becomes available. When started, upload and processing proceed, a warning not to leave the page is displayed, and a successful result can be downloaded or opened in the editor. |
| Actual — screenshot | **Available time: 1 hour**, selected file **“Ready to upload!”**, duration **26 minutes 16 seconds**, **“Start upload!”** button present. |
| Actual — tester report | Clicking **Start upload!** completes the upload and processing flow; a “do not leave the page” notice appears during processing; after completion, **Download** and **Open in editor** are available and work correctly. |
| Outcome | **Pass as reported by tester**, with UI evidence for the ready state; processing/completion is **reported but not screenshot- or log-verified** in this artifact. |

**Steps**
1. Set up an account with **1 hour** of available processing time and open **Your documents**.
2. Select a supported video of **26 minutes 16 seconds**.
3. Verify the **Ready to upload!** state and presence of **Start upload!**.
4. Click **Start upload!**.
5. Observe upload/processing and the warning not to navigate away.
6. Wait for a successful completion state.
7. Verify that the result can be downloaded and opened in the editor.

**State observations** (not a complete formal model): *File selected → Ready to upload → Upload/processing in progress → Completed → Download/Open in editor*. The blocked path is *File selected → Insufficient balance*. Additional failure, retry, navigation and cancellation transitions require separate tests.

## 4. Decision table — balance compared with media duration

| Rule | Available processing time | Video duration | Expected action | Evidence status |
| --- | --- | --- | --- | --- |
| DT-BAL-01 | 0 sec | 10 sec | Block upload, show insufficient-balance message and a **10 sec** shortage | **Observed** — screenshot |
| DT-BAL-02 | 3600 sec | 1576 sec (26:16) | Ready state; Start upload enabled; successful processing should yield output | **Observed ready state**; completion **tester-reported** |
| DT-BAL-03 | D − 1 sec | D sec | Proposed: block; show shortage of **1 second** | **Not Run**, requires confirmed comparison/rounding rules |
| DT-BAL-04 | D sec | D sec | Proposed: allow if the required balance equals the duration | **Not Run**, requires confirmed equality rule |
| DT-BAL-05 | D + 1 sec | D sec | Proposed: allow if no other constraints prevent it | **Not Run**, requires confirmed policy |

The third through fifth rows are **future BVA candidates**, not verified product rules. Also test how video duration and required processing time are rounded (full seconds versus minute rounding, multiple files, transcoding, and other potential rules).

## 5. Why this is useful as a portfolio artifact

- **Equivalence Partitioning:** distinguish **insufficient** and **sufficient** processing time relative to selected media, not just zero versus positive balance.
- **Decision Table Testing:** document different outcomes for combinations of balance and duration.
- **State Transition Testing:** capture the observed blocked, ready, processing and completed stages; expand only after recording actual transitions.
- **Requirement-based reporting:** separate a correctly displayed error state from a **potential UX defect** concerning missing top-up guidance.
- **Evidence discipline:** label what is visible in a screenshot, what was reported by a tester, and what still requires further execution.

## 6. Execution metadata to capture next

Browser/version, operating system/device, product build, test date, supported file type, exact test data duration, media processing requirements, evidence of processing warning and output, and any real defect ID. Use synthetic media and redact names/identifiers when publishing.

[← File upload overview](audiodoc-studio-file-upload.md) · [← Portfolio home](../README.md)
