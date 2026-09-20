## Moderation domain

Types representing policies, scores, decisions, and outcomes produced during moderation.

### Decision

The outcome of applying policy rules to a set of scores.

| Value       | Meaning                                                                                                         |
| ----------- | --------------------------------------------------------------------------------------------------------------- |
| `Approved`  | No score reaches the rejection threshold or the escalation band.                                                |
| `Rejected`  | At least one score is at or above the rejection threshold.                                                      |
| `Escalated` | No score reaches the rejection threshold, but at least one falls in the escalation band; human review required. |

### Verdict

The final disposition of a submission.

| Value      | Meaning                              |
| ---------- | ------------------------------------ |
| `Approved` | The submission may be published.     |
| `Rejected` | The submission may not be published. |

### DecisionSource

Who or what produced a verdict.

| Value           | Meaning                                                                    |
| --------------- | -------------------------------------------------------------------------- |
| `Policy`        | Produced automatically by applying the policy thresholds.                  |
| `Reviewer`      | Produced by a human reviewer.                                              |
| `TimeoutPolicy` | Produced by the policy's timeout verdict after the review deadline passed. |

### ScoreBand

An inclusive range of scores.

#### Properties

| Name    | Type   | Description                                   |
| ------- | ------ | --------------------------------------------- |
| `lower` | number | The lowest score in the band, in [0.0, 1.0].  |
| `upper` | number | The highest score in the band, in [0.0, 1.0]. |

### ModerationPolicy

The thresholds and deadline that govern moderation decisions.

#### Properties

| Name                  | Type                    | Description                                                                                           |
| --------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------- |
| `rejection_threshold` | number                  | Score at or above which a submission is rejected automatically.                                       |
| `escalation_band`     | [ScoreBand](#scoreband) | Score range, entirely below the rejection threshold, within which a submission requires human review. |
| `review_deadline`     | number                  | Seconds a review request may wait before the timeout verdict applies.                                 |
| `timeout_verdict`     | [Verdict](#verdict)     | The verdict applied when the review deadline passes.                                                  |

### ScoreSet

Scores computed across all moderation dimensions for a submission.

#### Properties

| Name          | Type   | Description                          |
| ------------- | ------ | ------------------------------------ |
| `toxicity`    | number | Toxicity score in [0.0, 1.0].        |
| `spam`        | number | Spam likelihood score in [0.0, 1.0]. |
| `hate_speech` | number | Hate speech score in [0.0, 1.0].     |

### ModerationOutcome

The final outcome of the moderation pipeline for a single submission.

#### Properties

| Name            | Type                              | Description                       |
| --------------- | --------------------------------- | --------------------------------- |
| `submission_id` | text                              | The ID of the submission.         |
| `verdict`       | [Verdict](#verdict)               | The final disposition.            |
| `decided_by`    | [DecisionSource](#decisionsource) | Who or what produced the verdict. |
