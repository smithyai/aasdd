## Link Checker

**AASDD:** v2
**Version:** 1.0.0

Accepts a document and returns a report indicating which of its links are alive, dead, or unreachable.

### Purpose

For anyone who publishes documents containing links and needs to know which of those links still work. It answers that question for one document at a time, so it can sit behind an editor, a build step, or a scheduled check without knowing anything about where documents come from.

### Non-Goals

- Does not crawl the pages a link points to; only the links in the submitted document are checked.
- Does not retry unreachable hosts or wait for them to recover.
- Does not store reports; every call is independent.

### Success Criteria

| Criterion                                                                                                   | Abilities       | Scenarios             |
| ----------------------------------------------------------------------------------------------------------- | --------------- | --------------------- |
| For every unique link in a submitted document, the caller learns whether it is alive, dead, or unreachable. | `CheckDocument` | `mixed-results`       |
| A document with no links yields an empty report, not an error.                                              | `CheckDocument` | `no-links`            |
| A document that cannot be parsed is rejected with an error rather than a partial report.                    | `CheckDocument` | `unparsable-document` |

### Invariants

- Every link extracted from the document appears in the report exactly once.
- The report's total count equals the number of unique links in the document.
