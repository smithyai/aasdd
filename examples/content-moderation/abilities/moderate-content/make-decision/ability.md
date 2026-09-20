## MakeDecision

Applies a moderation policy to a score set and produces a moderation decision.

### Inputs

| Name     | Type                                                                         | Description                                  |
| -------- | ---------------------------------------------------------------------------- | -------------------------------------------- |
| `scores` | [ScoreSet](../../../concepts/moderation/concept.md#scoreset)                 | Scores computed by `ScoreContent`.           |
| `policy` | [ModerationPolicy](../../../concepts/moderation/concept.md#moderationpolicy) | The thresholds and escalation band to apply. |

### Outputs

| Name       | Type                                                         | Description                           |
| ---------- | ------------------------------------------------------------ | ------------------------------------- |
| `decision` | [Decision](../../../concepts/moderation/concept.md#decision) | The outcome of the policy evaluation. |

### Invariants

- If any score in `scores` is greater than or equal to `policy.rejection_threshold`, `decision` is `Rejected`.
- Otherwise, if any score in `scores` is between `policy.escalation_band.lower` and `policy.escalation_band.upper` inclusive, `decision` is `Escalated`.
- Otherwise, `decision` is `Approved`.

### Failure Modes

| Failure         | Condition                                                                                                                                                                                                             | Effect                      |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `InvalidPolicy` | `policy.escalation_band.lower` is greater than `policy.escalation_band.upper`, or `policy.escalation_band.upper` is greater than or equal to `policy.rejection_threshold`, or any of the three is outside [0.0, 1.0]. | Error propagated to caller. |
