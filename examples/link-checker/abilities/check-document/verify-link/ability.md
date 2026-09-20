## VerifyLink

Checks whether a single link is reachable and returns its status.

### Inputs

| Name   | Type                                               | Description         |
| ------ | -------------------------------------------------- | ------------------- |
| `link` | [Link](../../../concepts/document/concept.md#link) | The link to verify. |

### Outputs

| Name     | Type                                                          | Description                            |
| -------- | ------------------------------------------------------------- | -------------------------------------- |
| `result` | [LinkResult](../../../concepts/results/concept.md#linkresult) | The verification outcome for the link. |

### Invariants

- `result.link` equals the input `link`.
- `result.status` is `Alive` when the link's target responds successfully.
- `result.status` is `Dead` when the link's target responds with an error.
- `result.status` is `Unreachable` when the link's target does not respond at all.

### Failure Modes

| Failure      | Condition                                    | Effect                      |
| ------------ | -------------------------------------------- | --------------------------- |
| `InvalidUrl` | `link.url` is not a syntactically valid URL. | Error propagated to caller. |
