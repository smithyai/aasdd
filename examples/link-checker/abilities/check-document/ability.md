## CheckDocument

Extracts all links from a document and verifies each one, returning a full report.

### Inputs

| Name       | Type                                                    | Description            |
| ---------- | ------------------------------------------------------- | ---------------------- |
| `document` | [Document](../../concepts/document/concept.md#document) | The document to check. |

### Outputs

| Name     | Type                                                               | Description                                             |
| -------- | ------------------------------------------------------------------ | ------------------------------------------------------- |
| `report` | [DocumentReport](../../concepts/results/concept.md#documentreport) | The verification results for all links in the document. |

### Invariants

- Every link in `document` appears in `report.results` exactly once.
- `report.total` equals the number of unique links in `document`.
- `report.alive`, `report.dead`, and `report.unreachable` sum to `report.total`.
- Each of `report.alive`, `report.dead`, and `report.unreachable` equals the number of entries in `report.results` with the corresponding status.

### Failure Modes

| Failure              | Condition                                                           | Effect                                             |
| -------------------- | ------------------------------------------------------------------- | -------------------------------------------------- |
| `UnparsableDocument` | `ExtractLinks` fails because the document content cannot be parsed. | Error propagated to caller; no report is produced. |

### Composition

| Step | Ability        | Consumes                           | Produces |
| ---- | -------------- | ---------------------------------- | -------- |
| 1    | `ExtractLinks` | `document` from parent             | `links`  |
| 2    | `VerifyLink`   | each `link` in `links` from step 1 | `result` |
| 3    | —              | every `result` from step 2         | `report` |
