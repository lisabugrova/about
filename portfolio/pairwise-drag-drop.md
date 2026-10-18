# Pairwise testing — drag-and-drop interaction (Audiodoc-style case)

> **Educational design example, not a record of Audiodoc test execution.** The team has described drag-and-drop interactions as a testing challenge. The exact interface, supported platforms, item counts, rules for valid/invalid drop zones, and actual requirements have **not been confirmed** here. All numbers and expectations below must be validated against the real feature specification before use in production.

## 1. Testing objective

Suppose a change affects how a user drags a content card into a target area in an interactive story task. We want to reduce the number of cross-configuration checks while ensuring that every **pair of factor values** is represented.

**Hypothetical factors:**

| Factor | Values |
| --- | --- |
| Browser | Chrome, Firefox, Safari |
| Device class | Desktop, Mobile |
| Number of draggable items | 3, 10 |
| Drop target | Valid, Invalid |

Full combination count = **3 × 2 × 2 × 2 = 24**.

Pairwise test-design goal: every possible pairing of values from different factors must appear in at least one test. For these factors there are **30 unique value pairs** (6 + 6 + 6 + 4 + 4 + 4).

## 2. Six-test pairwise covering array

| ID | Browser | Device | Items | Drop target | Example execution setup |
| --- | --- | --- | ---: | --- | --- |
| PW-01 | Chrome | Desktop | 3 | Valid | Desktop Chrome, pointer drag |
| PW-02 | Chrome | Mobile | 10 | Invalid | Android Chrome, touch drag |
| PW-03 | Firefox | Desktop | 3 | Invalid | Desktop Firefox, pointer drag |
| PW-04 | Firefox | Mobile | 10 | Valid | Android Firefox, touch drag |
| PW-05 | Safari | Desktop | 10 | Valid | macOS Safari, pointer drag |
| PW-06 | Safari | Mobile | 3 | Invalid | iOS Safari, touch drag |

**Coverage calculation:** this six-row set covers **all 30/30 unique value pairs** across the selected factors. It uses **6 instead of 24 configurations (75% fewer)**. This is **100% pairwise factor coverage**, not 100% requirements coverage, defects detected, execution success, or compatibility coverage.

The actual browser/OS versions and devices must be recorded. A resized desktop window is not a substitute for validating real touch behavior on mobile.

## 3. Reusable test procedure

**Prerequisites:** access to a test build with a configured draggable-item task; expected drag-and-drop behavior documented; a fixture containing the specified number of items.

1. Record test ID, build, OS/device model, browser version, and viewport.
2. Open the interactive task and check that the expected number of draggable items is present.
3. Select a draggable item and move it to a **valid** or **invalid** destination, as specified by the row.
4. Release the pointer/finger and observe the component's response.
5. Compare with the **confirmed** product requirement. Proposed example expectations (not confirmed for Audiodoc):
   - **Valid target:** item is accepted at the destination and the relevant state changes.
   - **Invalid target:** item is not assigned to the wrong destination; the UI gives appropriate feedback or restores the item.
6. Check that layout, touch/pointer interaction, and other items are usable.
7. Log **Pass / Fail / Blocked / Not Run**, plus evidence and any deviations.

**Worked example: PW-04** — On Firefox for Android, load the 10-item fixture, drag an item by touch into a valid area, and verify its final placement and task state against the confirmed specification.

**Worked example: PW-03** — In desktop Firefox, load the 3-item fixture, attempt to drop an item into an invalid area, and verify that the application handles the action according to its requirements.

## 4. What pairwise does NOT prove

- Pairwise is a **coverage technique for input/configuration interactions**. It does not prove all 24 combinations behave correctly.
- A bug requiring three or more interacting factor values may be missed.
- All six rows must still be executed; this document reports **test design only**.
- Real-world compatibility constraints can invalidate a matrix. Review browser, OS, touch input, feature flags, account state, and data setup first.
- Pairwise does not replace functional test scenarios, negative testing, exploratory testing, usability/accessibility checks, or regression based on historical defects.
- Add focused checks for **drag cancellation, repeated rapid dragging, scrolling while dragging, long lists, keyboard access, interrupted network requests, save/reload, and restoring task state** whenever those risks apply.

## 5. Adapting the example to an actual Audiodoc task

Before claiming project evidence, answer:

1. What specific task/user journey was changed? What does the product specify for correct and incorrect drops?
2. What are the supported browsers, OS versions, screen sizes, and input methods?
3. What are the **real** small/large item-count categories, or should we replace this factor?
4. Does any combination need exclusion (e.g. unavailable browser on a device or a task without invalid targets)?
5. Which high-impact edge cases deserve dedicated tests in addition to pairwise?

Update the matrix for these constraints, **regenerate and verify pair coverage**, execute in approved environments, and record actual results and defect links.

**Interview-ready distinction:** “I used pairwise to choose six representative configuration checks out of 24 possible combinations, covering all two-factor value interactions; I supplemented them with risk-based drag-and-drop scenarios. This file is a portfolio exercise, not a claim about production results.”

## References

- Lee Copeland, *A Practitioner's Guide to Software Test Design* — combinatorial and systematic test-design techniques.
- [ISTQB CTFL syllabus v4.0.1](https://istqb.org/wp-content/uploads/2024/11/ISTQB_CTFL_Syllabus_v4.0.1.pdf) — test-design techniques and test coverage concepts.

[← Test design workbook](test-design-workbook.md) · [← Portfolio home](../README.md)
