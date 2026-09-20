## Escalated and Reviewed

A submission with ambiguous scores is escalated to a human reviewer, who provides a final verdict before the review deadline.

> `Analyzing` → `Scoring` → `Deciding` → `Escalating` — ReviewReceived → `Complete`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` returns a `ScoreSet` with no score at or above `policy.rejection_threshold` and at least one score in the escalation band.
- `MakeDecision` returns `Escalated`.
- `EscalateForReview` routes the request to a reviewer, who responds before the deadline.
- The outcome has the reviewer's `verdict` and `decided_by` of `Reviewer`.

### Example

A user posts a comment that matches a flagged pattern. The toxicity score is 0.45 — within the escalation band of [0.3, 0.7]. A reviewer examines the context and determines the comment is harmful. Outcome: `Rejected`, decided by `Reviewer`.
