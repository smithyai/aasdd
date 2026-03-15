## CheckDocument

**Purpose:** Extracts all links from a document and verifies each one, returning a full report.

### Inputs

| Name       | Type                                                    | Description            |
| ---------- | ------------------------------------------------------- | ---------------------- |
| `document` | [Document](../../concepts/document/concept.md#document) | The document to check. |

### Outputs

| Name     | Type                                                               | Description                                             |
| -------- | ------------------------------------------------------------------ | ------------------------------------------------------- |
| `report` | [DocumentReport](../../concepts/results/concept.md#documentreport) | The verification results for all links in the document. |

### Invariants

- Every link in `document` appears in `report` exactly once.
- `report.total` equals the number of unique links in `document`.

### Failure Modes

| Failure              | Condition                                               | Effect                      |
| -------------------- | ------------------------------------------------------- | --------------------------- |
| `UnparsableDocument` | The document content cannot be parsed to extract links. | Error propagated to caller. |
