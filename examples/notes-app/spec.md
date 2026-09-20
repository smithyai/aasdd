## Notes App

**AASDD:** v2
**Version:** 0.3.0

A personal notes application that captures notes, finds them again by text and tag, and checks the links they contain.

### Purpose

For people who keep working notes in plain text and lose track of them. It exists so that a note written today can be found in seconds months later, and so that links pasted into notes do not rot unnoticed.

### Non-Goals

- Does not synchronize notes between devices.
- Does not support collaborative editing.
- Does not render rich media; note bodies are plain text.

### Success Criteria

| Criterion                                                                   | Abilities                    | Scenarios          |
| --------------------------------------------------------------------------- | ---------------------------- | ------------------ |
| A note captured with a title and body is retrievable by any word in either. | `CaptureNote`, `BrowseNotes` | `capture-and-find` |
| A note can be found by any tag attached to it.                              | `BrowseNotes`                | —                  |
| Every link in a note can be checked, and dead links are reported.           | `CheckNoteLinks`             | —                  |

### Invariants

- A note's `id` never changes after capture.
- Every note appears at most once in any list view.
