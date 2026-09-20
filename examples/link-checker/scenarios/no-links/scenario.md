## No Links

A document with no links produces an empty report.

> `CheckDocument` → `ExtractLinks`

- `ExtractLinks` returns an empty list.
- `VerifyLink` is never invoked.
- `report.results` is empty and `report.total` is 0.
