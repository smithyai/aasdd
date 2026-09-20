## RenderNoteList

Turns a list of notes into the rows and empty state the notes screen displays.

### Inputs

| Name      | Type                                                   | Description                            |
| --------- | ------------------------------------------------------ | -------------------------------------- |
| `matches` | list of [Note](../../../concepts/note/concept.md#note) | The notes to display, already ordered. |

### Outputs

| Name   | Type                                                           | Description                              |
| ------ | -------------------------------------------------------------- | ---------------------------------------- |
| `view` | [NoteListView](../../../concepts/note/concept.md#notelistview) | The rows to display, or the empty state. |

### Invariants

- `view.rows` has one entry per note in `matches`, in the same order.
- Each row's `title` equals its note's `title`, or the first line of the note's `body` when the title is empty.
- Each row's `tags` equals its note's `tags`.
- When `matches` is empty, `view.empty` is `true`, `view.rows` is empty, and `view.prompt` is present and invites the user to capture a note.
- When `matches` is not empty, `view.empty` is `false` and `view.prompt` is absent.

### Failure Modes

_None._
