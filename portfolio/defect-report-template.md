# Example defect report

Fictional example, not a reported Audiodoc defect.

| Field | Value |
| --- | --- |
| ID | DEMO-EDIT-01 |
| Summary | Saved correction disappears after reopening a transcript |
| Build / environment | DEMO-1; Chrome desktop; synthetic account and media |
| Feature | Edit → save → reload |
| Impact | The user may trust the save confirmation and export incorrect text |
| Proposed severity | High if reproducible in a supported workflow: loss of user work |
| Proposed priority | Block a release that changes saving; confirm affected scope with the team |
| Related check | Editor persistence and regression checks |

## Reproduce

The transcript contains “We meet on Monday.” Its owner is signed in and can edit. The browser is online, loading has finished and there are no unsaved changes.

1. Replace “Monday” with “Tuesday”.
2. Wait for the documented save mechanism to finish; record its visible status and response.
3. Reload and reopen the same document.
4. Inspect the sentence.

| Expected | Actual in this example |
| --- | --- |
| “We meet on Tuesday.” remains after reload. | “We meet on Monday.” returns despite the earlier save confirmation. |

## Evidence and follow-up

For a real report, attach a recording of edit → save → reload, the test-document ID, exact build/browser, timestamps, reproduction rate and relevant responses. Remove private data.

- Compare the save acknowledgement with the stored value; check for a delayed load.
- Check whether the issue affects a particular editor, document generation or edit type. Leave the cause open until verified.
- After a fix, repeat these steps and check punctuation, speaker changes and navigation.
- Record the retest build and result. Close the issue only after verifying the fix.

[Regression](regression-and-release.md) · [Design index](test-design-workbook.md) · [Profile](../README.md)
