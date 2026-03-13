## MakeDecision

**Purpose:** Applies configured policy thresholds to a score set and produces a moderation decision.

### Inputs

| Name | Type | Description |
| --- | --- | --- |
| `scores` | [ScoreSet](../../../concepts/moderation/concept.md#scoreset) | Scores computed by `ScoreContent`. |

### Outputs

| Name | Type | Description |
| --- | --- | --- |
| `decision` | [Decision](../../../concepts/moderation/concept.md#decision) | The outcome of the policy evaluation. |

### Invariants

- If any score exceeds the rejection threshold, `decision` is `Rejected`.
- If no score exceeds any threshold, `decision` is `Approved`.
- `Escalated` is returned only when at least one score falls in the configured escalation band.

### Failure Modes

| Failure | Condition | Effect |
| --- | --- | --- |
| `InsufficientScores` | `scores` contains no dimensions covered by the configured policy. | Error propagated to caller. |