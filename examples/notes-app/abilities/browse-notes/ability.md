## BrowseNotes

Finds the notes matching a query and presents them as a list the user can scan.

### Inputs

| Name    | Type                                                  | Description                                     |
| ------- | ----------------------------------------------------- | ----------------------------------------------- |
| `query` | [NoteQuery](../../concepts/note/concept.md#notequery) | Free text and tags to match; both may be empty. |

### Outputs

| Name   | Type                                                        | Description          |
| ------ | ----------------------------------------------------------- | -------------------- |
| `view` | [NoteListView](../../concepts/note/concept.md#notelistview) | The list to display. |

### Invariants

- `view.rows` is ordered by the matching notes' `updated_at`, newest first.
- `view.empty` is `true` if and only if `view.rows` is empty.

### Failure Modes

_Pending._

### Composition

| Step | Ability          | Consumes              | Produces  |
| ---- | ---------------- | --------------------- | --------- |
| 1    | `SearchNotes`    | `query` from parent   | `matches` |
| 2    | `RenderNoteList` | `matches` from step 1 | `view`    |
