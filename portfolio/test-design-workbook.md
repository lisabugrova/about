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
Assumptions: cached story list can be read offline; a location pin appears only with permission granted and a location fix; location refusal never blocks the story list.

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

## 5. Pairwise coverage thinking
Potential dimensions: browser (Chrome / Firefox / Safari), viewport (mobile / desktop), network (online / offline), location permission (granted / denied). A full Cartesian combination produces 3 × 2 × 2 × 2 = **24** combinations. Generate a pairwise covering array using a suitable tool; **do not label a manually selected subset as verified pairwise coverage** without checking all value pairs. Use risk to add high-impact scenarios beyond pairwise coverage.

## Traceability and review
Map each scenario to a requirement ID once real requirements are available; record environment, build, expected result, actual result, and evidence on execution. These tables demonstrate *design*, not a test run.

[← Portfolio home](../README.md)
