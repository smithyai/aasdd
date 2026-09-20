## InteractionChannel

### Context

`CaptureNote`, `BrowseNotes`, and `CheckNoteLinks` are root abilities whose inputs come from the person using the application.

### Requirement

A way for the user to submit a `NoteDraft`, a `NoteQuery`, and a request to check a note's links, and to see the resulting `Note`, `NoteListView`, and link report.

### Decision

A local desktop application. Root-level inputs arrive as events from the application's own screens; there is no network interface.
