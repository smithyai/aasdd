## ReviewerChannel

### Context

`EscalateForReview` hands a submission to a human and waits for a verdict, or for `policy.review_deadline` to pass. The spec defines the outcome but not how reviewers receive requests or return verdicts.

### Requirement

A way to deliver a review request carrying the `Submission` and `ScoreSet` to a reviewer, to receive a `Verdict` back, to learn at request time that no reviewer can accept it, and to enforce the deadline.

### Decision

A work queue consumed by the reviewer console. Enqueueing fails with `NoReviewerAvailable` when the queue reports no active reviewers; a request that no reviewer answers within the deadline yields the `ReviewTimedOut` outcome.
