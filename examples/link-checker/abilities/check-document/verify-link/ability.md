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

### Failure Modes

| Failure           | Condition                     | Effect                                      |
| ----------------- | ----------------------------- | ------------------------------------------- |
| `UnreachableHost` | The host cannot be contacted. | Returns a result with status `Unreachable`. |

### Idempotency

Invoking with the same link may return different results if the link's target changes between invocations. Not idempotent.
