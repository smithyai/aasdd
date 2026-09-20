## Mixed Results

A document containing alive, dead, and unreachable links produces a report that classifies each one.

> `CheckDocument` → `ExtractLinks` → `VerifyLink`

- `ExtractLinks` returns three unique links.
- `VerifyLink` is invoked once per link and returns `Alive`, `Dead`, and `Unreachable` respectively.
- `report.total` is 3, and `report.alive`, `report.dead`, and `report.unreachable` are each 1.

### Example

A blog post links to its author's homepage, a retired documentation page, and a host that no longer resolves. The report lists all three with their statuses, so the author can fix the two that failed.
