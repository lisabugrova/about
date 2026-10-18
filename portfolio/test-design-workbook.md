# Test design workbook — fictional audio-story web app

**Type:** original educational exercise. **Execution status:** not executed. **Production affiliation:** none.

## Assumed product rules
A mobile-friendly web application lets visitors open a public story, start/pause its audio, and save a listening position locally. The user may choose playback speed from **0.75×, 1×, 1.25×, 1.5×, 2×**. A story can be **available**, **unavailable**, or **loading**. Location permission is optional; when denied, users can browse a list of stories without a map position. 
## 1. Equivalence partitioning
For a hypothetical story-search field:
- **Valid:** 1–80 visible characters.
- **Invalid:** empty input; 81+ visible characters.
- **Unspecified:** whitespace-only, Unicode normalization, leading/trailing spaces, emoji, and server-side limits. These require clarification.

| ID | Partition | Example input | Expected result |
| --- | --- | --- | --- |
| EP-01 | Valid length | `history` | Query accepted |
| EP-02 | Empty | empty string | Input rejected or guidance shown |
| EP-03 | Too long | 81 characters | Input rejected with understandable feedback |

**Oracle note:** These outcomes depend on the assumed requirements; clarify before actual execution.

## 2. Boundary value analysis
Test 0, 1, 2 and 79, 80, 81 characters for an inclusive 1–80-character constraint. Include multibyte characters and whitespace as *separate exploratory risks*; do not equate bytes, code points, and perceived characters.

| ID | Input length | Expected |
| --- | ---: | --- |
| BVA-01 | 0 | Reject |
| BVA-02 | 1 | Accept |
| BVA-03 | 2 | Accept |
| BVA-04 | 79 | Accept |
| BVA-05 | 80 | Accept |
| BVA-06 | 81 | Reject |

## 3. Decision table: location and connectivity
Cached story list can be read offline; a location pin appears only with permission granted and a location fix; location refusal never blocks the story list.

| Rule | Network available | List cache available | Location granted | Expected |
| --- | --- | --- | --- | --- |
| D-01 | Yes | Either | Yes | Fetch list; location-enhanced map view where available |
| D-02 | Yes | Either | No | Fetch list; no user-location pin |
| D-03 | No | Yes | Yes | Cached list with offline notice; location behavior depends on device fix |
| D-04 | No | Yes | No | Cached list with offline notice; no user-location pin |
| D-05 | No | No | Either | Offline empty state with retry guidance |

**Rule D-03 is intentionally incomplete:** confirm whether the product supports offline maps before assigning a map expectation.

## 4. State transition testing: audio player
States: **Idle → Loading → Playing ↔ Paused → Ended**. Error can occur from Loading or Playing. The user can retry after an error.

| ID | Start state | Event | Expected state |
| --- | --- | --- | --- |
| ST-01 | Idle | Play | Loading |
| ST-02 | Loading | Media ready | Playing |
| ST-03 | Playing | Pause | Paused |
| ST-04 | Paused | Resume | Playing |
| ST-05 | Playing | Track ends | Ended |
| ST-06 | Loading | Network failure | Error |
| ST-07 | Error | Retry | Loading |

Also explore invalid transitions, repeated taps, slow responses, backgrounding, and interruption by incoming audio.

## 5. Pairwise testing: practical drag-and-drop example

**Audiodoc-style educational scenario, not a documented production test run.** For a hypothetical drag-and-drop task, vary **browser** (Chrome / Firefox / Safari), **device** (desktop / mobile), **draggable item count** (3 / 10), and **drop target** (valid / invalid). All these values are *assumptions* to validate against the actual feature.

There are **3 × 2 × 2 × 2 = 24** full configurations, but this verified pairwise covering array needs **6 configurations** and covers all **30 distinct value pairs**:

| ID | Browser | Device | Items | Drop target |
| --- | --- | --- | ---: | --- |
| PW-01 | Chrome | Desktop | 3 | Valid |
| PW-02 | Chrome | Mobile | 10 | Invalid |
| PW-03 | Firefox | Desktop | 3 | Invalid |
| PW-04 | Firefox | Mobile | 10 | Valid |
| PW-05 | Safari | Desktop | 10 | Valid |
| PW-06 | Safari | Mobile | 3 | Invalid |

Each row is an **execution configuration**, not a test result. On the selected device/browser, open the specified task, drag an item to the chosen target type, and check placement, visual feedback, and task state against confirmed requirements. Record environment, build, pass/fail/blocked status, and evidence.

See **[full pairwise drag-and-drop case study](pairwise-drag-drop.md)** for prerequisites, exact example steps, pair-coverage calculation, risk-based additional tests, and limitations. Pairwise is **not** a substitute for high-impact edge cases, nor proof that every 3-way interaction is covered.

## Traceability and review
Map each scenario to a requirement ID once real requirements are available; record environment, build, expected result, actual result, and evidence on execution. These tables demonstrate *design*, not a test run.

[← Portfolio home](../README.md)
