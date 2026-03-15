## Escalated and Reviewed

A submission with ambiguous scores is escalated to a human reviewer, who provides a final decision before the review deadline.

> `Analyzing` → `Scoring` → `Deciding` → `Escalating` → `Complete`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` returns a `ScoreSet` with at least one score in the escalation band.
- `MakeDecision` returns `Escalated`.
- `EscalateForReview` routes the request to a reviewer, who responds before the deadline.
- Outcome has `decided_by` of `"reviewer"`.

### Example

A user posts a comment that matches a flagged pattern. The toxicity score is 0.45 — within the escalation band of [0.3, 0.7]. A reviewer examines the context and determines the comment is harmful. Outcome: `Rejected` by `"reviewer"`.
