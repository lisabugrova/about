# Illustrative defect reports

**Status: fictional examples only.** Neither issue has been observed in production; no real execution or defect history is implied. Designed to show report quality.

## SAMPLE-BUG-001 — Audio continues playing after navigation

- **Severity (illustrative):** Major — unexpected audio may affect user control
- **Priority:** to be agreed with the product team
- **Environment:** hypothetical mobile browser; version/build to capture on execution
- **Preconditions:** story A loaded, audio starts playing
- **Steps to reproduce:**
  1. Open story A.
  2. Start playback.
  3. Navigate to story B through the application.
- **Expected:** Playback follows the documented navigation policy; assuming one active player, story A stops or is clearly controlled.
- **Actual (fictional):** Story A continues playing while controls for story B are displayed.
- **Evidence to collect:** screen recording, build, device/browser, media/network logs, repeatability.
- **Open question:** Is background playback intentional? Confirm specification before filing in a real project.

## SAMPLE-BUG-002 — Location denial blocks non-location browsing

- **Severity (illustrative):** Major — core content becomes inaccessible
- **Priority:** to be determined
- **Preconditions:** fresh permissions; network online
- **Steps:**
  1. Open the map/story view.
  2. Deny location permission.
  3. Select the list-browsing option.
- **Expected (assumed requirement):** The user can browse stories without providing location.
- **Actual (fictional):** The page displays a blocking error and no list.
- **Evidence to collect:** permission state, screenshot, console and network messages, browser/OS details, reproducibility.
- **Potential regression:** permission requested again; disabled location services; denied after previously allowed; mobile and desktop.

## Review checklist for actual defect tickets
A good report describes a single observable deviation; includes an executable sequence, precise expected/actual results, affected build and environment, impact, and links to appropriate evidence. Distinguish observed behavior from hypotheses. Never put secrets, personal data, or private URLs in public issues.

[← Portfolio home](../README.md)
