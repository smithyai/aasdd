## SearchNotes

Returns every stored note that contains all of the query's words and carries all of its tags.

### Inputs

| Name    | Type                                                     | Description                 |
| ------- | -------------------------------------------------------- | --------------------------- |
| `query` | [NoteQuery](../../../concepts/note/concept.md#notequery) | The text and tags to match. |

### Outputs

| Name      | Type                                                   | Description                       |
| --------- | ------------------------------------------------------ | --------------------------------- |
| `matches` | list of [Note](../../../concepts/note/concept.md#note) | The matching notes, newest first. |

### Invariants

_Pending._

### Failure Modes

_Pending._
