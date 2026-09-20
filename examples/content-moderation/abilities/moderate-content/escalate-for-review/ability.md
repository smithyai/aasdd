## EscalateForReview

Sends a review request to a human reviewer and returns the reviewer's outcome, or the policy's timeout outcome if the review deadline passes.

### Inputs

| Name         | Type                                                                         | Description                                                 |
| ------------ | ---------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `submission` | [Submission](../../../concepts/submission/concept.md#submission)             | The submission requiring review.                            |
| `scores`     | [ScoreSet](../../../concepts/moderation/concept.md#scoreset)                 | Scores included in the review request for reviewer context. |
| `policy`     | [ModerationPolicy](../../../concepts/moderation/concept.md#moderationpolicy) | Supplies the review deadline and the timeout verdict.       |

### Outputs

| Name      | Type                                                                           | Description                                                        |
| --------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `outcome` | [ModerationOutcome](../../../concepts/moderation/concept.md#moderationoutcome) | The outcome determined by the reviewer, or by the timeout verdict. |

### Invariants

- `outcome.submission_id` equals `submission.id`.
- When a reviewer's verdict is received within `policy.review_deadline` seconds of the request, `outcome.verdict` is that verdict and `outcome.decided_by` is `Reviewer`.
- When no verdict is received within `policy.review_deadline` seconds, `outcome.verdict` is `policy.timeout_verdict` and `outcome.decided_by` is `TimeoutPolicy`.
- Each invocation creates a new review request; earlier requests for the same submission are not reused.

### Failure Modes

| Failure               | Condition                                                                                  | Effect                      |
| --------------------- | ------------------------------------------------------------------------------------------ | --------------------------- |
| `NoReviewerAvailable` | The review channel reports that no reviewer can accept the request at the time it is made. | Error propagated to caller. |
