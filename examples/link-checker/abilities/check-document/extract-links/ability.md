## ExtractLinks

Parses a document and returns all unique links found within it.

### Inputs

| Name       | Type                                                       | Description            |
| ---------- | ---------------------------------------------------------- | ---------------------- |
| `document` | [Document](../../../concepts/document/concept.md#document) | The document to parse. |

### Outputs

| Name    | Type                                                       | Description                             |
| ------- | ---------------------------------------------------------- | --------------------------------------- |
| `links` | list of [Link](../../../concepts/document/concept.md#link) | All unique links found in the document. |

### Invariants

- `links` contains no duplicates.
- If the document contains no links, `links` is empty.

### Failure Modes

| Failure              | Condition                              | Effect                      |
| -------------------- | -------------------------------------- | --------------------------- |
| `UnparsableDocument` | The document content cannot be parsed. | Error propagated to caller. |
