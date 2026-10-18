# Defect Reporting — Worked Example

**Artifact type:** illustrative example. The scenario below is fictional and is not a reported Audiodoc defect or a claim of a defect discovered during employment.

## Example: a transcript correction disappears after reload

| Field | Example value |
| --- | --- |
| ID | DEMO-EDIT-01 |
| Summary | Saved correction reverts after reopening a transcript |
| Build / environment | Fictional training build DEMO-1; Chrome desktop; synthetic account and media |
| Feature | Transcript edit → save → reload |
| Impact | A user can believe a correction was saved and later export incorrect text |
| Proposed severity | High if reproducible in a supported workflow: loss of user work |
| Proposed priority | Release blocker for a change affecting save behavior; confirm affected scope with the team |
| Related check | Editor persistence and corresponding regression coverage |

### Preconditions

A synthetic transcript contains “We meet on Monday.” The signed-in owner can edit it. Start with an online browser, completed initial load and no unsaved edits.

### Reproduction

1. Replace “Monday” with “Tuesday”.
2. Wait until the documented save mechanism finishes; record visible status and the relevant response.
3. Reload the page and reopen the same document.
4. Inspect the edited sentence.

### Expected

The saved sentence is “We meet on Tuesday.” Reloading does not restore the earlier value.

### Hypothetical actual result

The editor returns to “We meet on Monday.” The example assumes the UI previously indicated successful saving.

### Evidence packet

For a real report, attach a short sanitized before/edit/save/reload recording, document fixture ID, exact build/browser, timestamps, reproduction rate and relevant redacted response details. This fictional example intentionally contains no invented screenshot, request log or execution count.

### Investigation and fix verification

- Compare visible save acknowledgement with the persisted value and any delayed load.
- Check whether the result is specific to one editor, document generation or edit type.
- After a fix, repeat the exact reproduction and test adjacent punctuation, speaker changes and navigation.
- Record the retest build and outcome; close only after the changed behavior is verified.

## Reusable report structure

**Identity and environment → impact → preconditions/data → steps → expected → actual → evidence → reproducibility → fix verification.**

A report should let another person reproduce the behavior without guessing. Keep severity (impact) separate from priority (when to address it). Unknown cause is acceptable; an unsupported diagnosis is not necessary.

[Regression](regression-and-release.md) · [Design index](test-design-workbook.md) · [Profile](../README.md)

