## ModerateContent

Drives a submission through analysis, scoring, decision, and optional human review, returning a final moderation outcome.

### Inputs

| Name         | Type                                                                      | Description                                                    |
| ------------ | ------------------------------------------------------------------------- | -------------------------------------------------------------- |
| `submission` | [Submission](../../concepts/submission/concept.md#submission)             | The content to moderate.                                       |
| `policy`     | [ModerationPolicy](../../concepts/moderation/concept.md#moderationpolicy) | The thresholds, escalation band, and review deadline in force. |

### Outputs

| Name      | Type                                                                        | Description                           |
| --------- | --------------------------------------------------------------------------- | ------------------------------------- |
| `outcome` | [ModerationOutcome](../../concepts/moderation/concept.md#moderationoutcome) | The final verdict and its provenance. |

### Invariants

- `outcome.submission_id` equals `submission.id`.
- `outcome.decided_by` is `Policy` if and only if `MakeDecision` returned `Approved` or `Rejected`, and `outcome.verdict` is then that decision.
- `outcome.decided_by` is `Reviewer` or `TimeoutPolicy` if and only if `MakeDecision` returned `Escalated`.
- An invocation that does not fail produces exactly one `outcome`.

### Failure Modes

| Failure                   | Condition                                              | Effect                                              |
| ------------------------- | ------------------------------------------------------ | --------------------------------------------------- |
| `AnalysisError`           | `AnalyzeContent` fails.                                | Error propagated to caller; no outcome is produced. |
| `ScoringModelUnavailable` | `ScoreContent` fails.                                  | Error propagated to caller; no outcome is produced. |
| `InvalidPolicy`           | `MakeDecision` fails because `policy` is inconsistent. | Error propagated to caller; no outcome is produced. |
| `NoReviewerAvailable`     | `EscalateForReview` fails.                             | Error propagated to caller; no outcome is produced. |
