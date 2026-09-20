## Review Timed Out

A submission is escalated, but no reviewer responds before the deadline, so the policy's timeout verdict applies.

> `Analyzing` → `Scoring` → `Deciding` → `Escalating` — ReviewTimedOut → `Complete`

- `MakeDecision` returns `Escalated`.
- `EscalateForReview` enqueues the request successfully, but no verdict arrives within `policy.review_deadline` seconds.
- The outcome has `verdict` equal to `policy.timeout_verdict` and `decided_by` of `TimeoutPolicy`.

### Example

A comment with a spam score of 0.6 is escalated at a busy hour. The policy's review deadline is 900 seconds and its timeout verdict is `Rejected`. No reviewer picks it up in time, and the comment is held back rather than published unreviewed.
