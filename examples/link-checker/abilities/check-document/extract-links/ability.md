## ExtractLinks

Parses a document and returns all unique links found within it.

### Inputs

| Name       | Type                                                       | Description            |
| ---------- | ---------------------------------------------------------- | ---------------------- |
| `document` | [Document](../../../concepts/document/concept.md#document) | The document to parse. |

### Outputs

| Name    | Type                                                       | Description                                                           |
| ------- | ---------------------------------------------------------- | --------------------------------------------------------------------- |
| `links` | list of [Link](../../../concepts/document/concept.md#link) | All unique links found in the document, in order of first appearance. |

### Invariants

- No two entries in `links` have the same `url`.
- If the document contains no links, `links` is empty.
- Every entry in `links` has a syntactically valid `url`.

### Failure Modes

| Failure              | Condition                              | Effect                      |
| -------------------- | -------------------------------------- | --------------------------- |
| `UnparsableDocument` | The document content cannot be parsed. | Error propagated to caller. |

### Idempotency

Invoking with an identical `document` returns an identical `links` list, in the same order.
