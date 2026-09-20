## NoteStorage

### Context

`CaptureNote` stores notes and `SearchNotes` reads them back. The spec defines the `Note` type and the invariants on capture and search, but not where notes live between calls.

### Requirement

Durable storage for notes on the user's machine that survives restarts, supports lookup by any word in the title or body and by tag, and reports write failures so that `StorageUnavailable` can be raised.

### Options

- A single embedded database with a full-text index: fastest search, but opaque to other tools.
- One plain-text file per note in a user-visible folder: readable by any editor, but search must be built or indexed separately.
- A hosted database: contradicts the non-goal of not synchronizing between devices.

### Decision

_Open._
