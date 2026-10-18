# Elizaveta Bugrova

**Manual QA engineer** · Tbilisi, Georgia  
[Telegram](https://t.me/lizavetamirr) · [GitHub](https://github.com/lisabugrova) · [Русский](ru/README.md)

I've worked in QA since 2024, testing web applications. My work includes functional and regression testing, release checks, test documentation and knowledge transfer.

## Audiodoc Studio

Audiodoc Studio transcribes audio and video and lets users edit the text alongside the recording. The work samples here focus on file uploads and processing-time balance.

A few details from the test design:

- To check the ten-file limit, use enough balance for all files. Otherwise, a balance error can hide a queue-limit problem.
- Check the exact file-size boundary, not just a file described as “large”.
- Check that a file can be selected with zero balance while processing remains blocked.

[Read the Audiodoc case study](portfolio/audiodoc-studio-case-study.md)

## Work and references

- **[CV](CV.md)** — experience and skills.
- **[Upload tests](portfolio/audiodoc-studio-file-upload.md)** and **[balance tests](portfolio/audiodoc-studio-balance-validation.md)** — rules, data and expected results.
- **[Execution record](portfolio/test-execution-report.md)** — two examples from the original portfolio.
- **[Full portfolio](portfolio/test-design-workbook.md)** — editing, exports, payments, access, regression and handover references.

The extended suite is a collaborative reference; [the portfolio notes explain its authorship and scope](portfolio/test-design-workbook.md#about-these-materials).

I'm interested in manual QA roles in product engineering teams, including international teams.
