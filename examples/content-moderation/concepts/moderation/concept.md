## Moderation domain

Types representing scores, decisions, and outcomes produced during moderation.

### Decision

The outcome of applying policy rules to a set of scores.

| Value | Meaning |
| --- | --- |
| `Approved` | The submission passes all policy thresholds. |
| `Rejected` | The submission exceeds at least one rejection threshold. |
| `Escalated` | At least one score falls in the escalation band; human review required. |

### ScoreSet

Scores computed across all moderation dimensions for a submission.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `toxicity` | number | Toxicity score in [0.0, 1.0]. |
| `spam` | number | Spam likelihood score in [0.0, 1.0]. |
| `hate_speech` | number | Hate speech score in [0.0, 1.0]. |

### ModerationOutcome

The final outcome of the moderation pipeline for a single submission.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `submission_id` | text | The ID of the submission. |
| `decision` | [Decision](#decision) | The final decision: `Approved` or `Rejected`. |
| `decided_by` | text | Who or what made the decision: `"policy"`, `"reviewer"`, or `"timeout-policy"`. |