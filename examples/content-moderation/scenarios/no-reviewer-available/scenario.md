## No Reviewer Available

A submission is escalated, but the review channel has no reviewer to accept it, so processing fails.

> `Analyzing` → `Scoring` → `Deciding` → `Escalating` — NoReviewerAvailable → `Failed`

- `MakeDecision` returns `Escalated`.
- `EscalateForReview` fails with `NoReviewerAvailable` because the review channel reports no active reviewers.
- `ModerateContent` propagates `NoReviewerAvailable` to the caller and produces no outcome.
