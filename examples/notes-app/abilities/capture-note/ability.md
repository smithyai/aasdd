## CaptureNote

Stores a new note from a draft and returns it with its identity and timestamps assigned.

### Inputs

| Name    | Type                                                  | Description                         |
| ------- | ----------------------------------------------------- | ----------------------------------- |
| `draft` | [NoteDraft](../../concepts/note/concept.md#notedraft) | The title, body, and tags to store. |

### Outputs

| Name   | Type                                        | Description      |
| ------ | ------------------------------------------- | ---------------- |
| `note` | [Note](../../concepts/note/concept.md#note) | The stored note. |

### Invariants

- `note.title` equals `draft.title` and `note.body` equals `draft.body`.
- `note.tags` equals `draft.tags` with duplicates removed, in first-seen order.
- `note.created_at` equals `note.updated_at`.
- `note.id` differs from the `id` of every previously captured note.

### Failure Modes

| Failure              | Condition                                                    | Effect                                         |
| -------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| `EmptyNote`          | `draft.title` and `draft.body` are both empty or whitespace. | Error propagated to caller; nothing is stored. |
| `StorageUnavailable` | The note store cannot be written.                            | Error propagated to caller; nothing is stored. |
