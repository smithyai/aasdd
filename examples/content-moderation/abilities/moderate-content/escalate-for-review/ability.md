## EscalateForReview

**Purpose:** Sends a review request to a human reviewer and returns the reviewer's moderation outcome, or a fallback outcome if the review deadline passes.

### Inputs

| Name | Type | Description |
| --- | --- | --- |
| `submission` | [Submission](../../../concepts/submission/concept.md#submission) | The submission requiring review. |
| `scores` | [ScoreSet](../../../concepts/moderation/concept.md#scoreset) | Scores to include in the review request for reviewer context. |

### Outputs

| Name | Type | Description |
| --- | --- | --- |
| `outcome` | [ModerationOutcome](../../../concepts/moderation/concept.md#moderationoutcome) | The outcome determined by the reviewer, or by timeout policy. |

### Invariants

- `outcome.submission_id` equals `submission.id`.
- `outcome.decision` is `Approved` or `Rejected` — never `Escalated`.
- `outcome.decided_by` is `"reviewer"` when a human review was received, or `"timeout-policy"` when the deadline expired.

### Failure Modes

| Failure | Condition | Effect |
| --- | --- | --- |
| `NoReviewerAvailable` | No reviewer is available to accept the request. | Error propagated to caller. |

### Idempotency

Invoking with the same submission a second time creates a new review request. Not idempotent.