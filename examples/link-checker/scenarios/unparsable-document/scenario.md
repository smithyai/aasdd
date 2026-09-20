## Unparsable Document

A document whose content cannot be parsed is rejected before any link is verified.

> `CheckDocument` → `ExtractLinks` — UnparsableDocument → `CheckDocument`

- `ExtractLinks` fails with `UnparsableDocument`.
- `VerifyLink` is never invoked.
- `CheckDocument` propagates `UnparsableDocument` to the caller and produces no report.
