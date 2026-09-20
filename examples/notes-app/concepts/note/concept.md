## Note domain

Types representing notes, the drafts they are captured from, queries over them, and the list view that presents them.

### NoteDraft

The user-supplied content of a note before it is stored.

#### Properties

| Name    | Type         | Description                             |
| ------- | ------------ | --------------------------------------- |
| `title` | text         | The title; may be empty.                |
| `body`  | text         | The plain-text body; may be empty.      |
| `tags`  | list of text | Tags to attach; may contain duplicates. |

### Note

A stored note.

#### Properties

| Name         | Type         | Description                            |
| ------------ | ------------ | -------------------------------------- |
| `id`         | text         | Unique identifier assigned at capture. |
| `title`      | text         | The title; may be empty.               |
| `body`       | text         | The plain-text body.                   |
| `tags`       | list of text | The note's tags, without duplicates.   |
| `created_at` | timestamp    | When the note was captured.            |
| `updated_at` | timestamp    | When the note was last changed.        |

### NoteQuery

Criteria for finding notes.

#### Properties

| Name   | Type         | Description                                                                |
| ------ | ------------ | -------------------------------------------------------------------------- |
| `text` | text         | Words that must all appear in the title or body; empty matches every note. |
| `tags` | list of text | Tags a note must all carry; empty matches every note.                      |

### NoteRow

One entry in the notes list.

#### Properties

| Name         | Type         | Description                              |
| ------------ | ------------ | ---------------------------------------- |
| `note_id`    | text         | The `id` of the note the row represents. |
| `title`      | text         | The text shown as the row's title.       |
| `tags`       | list of text | The note's tags.                         |
| `updated_at` | timestamp    | When the note was last changed.          |

### NoteListView

What the notes screen displays.

#### Properties

| Name     | Type                        | Description                                                                    |
| -------- | --------------------------- | ------------------------------------------------------------------------------ |
| `rows`   | list of [NoteRow](#noterow) | The rows to display, in display order.                                         |
| `empty`  | boolean                     | Whether there is nothing to display.                                           |
| `prompt` | optional text               | Text inviting the user to capture a note; present only when the list is empty. |
