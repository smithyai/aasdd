## Capture and Find

A note captured with a distinctive word in its body is found by searching for that word.

> `CaptureNote` → `BrowseNotes` → `SearchNotes` → `RenderNoteList`

- `CaptureNote` stores the note and returns it with an `id`.
- `SearchNotes` returns that note, and only that note, for a query containing the distinctive word.
- `RenderNoteList` returns one row whose `title` is the note's title.

### Example

The user writes a note titled "Standup" whose body mentions the quarterly roadmap. Two weeks later they type "roadmap" into the search box. The list shows exactly one row, "Standup", and opening it shows the note.
