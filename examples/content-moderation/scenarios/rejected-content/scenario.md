## Rejected Content

A submission with a score at or above the rejection threshold is rejected by policy without escalation, even if another score falls in the escalation band.

> `Analyzing` → `Scoring` → `Deciding` → `Complete`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` returns a `ScoreSet` with at least one score at or above `policy.rejection_threshold`.
- `MakeDecision` returns `Rejected`.
- The outcome has `verdict` of `Rejected` and `decided_by` of `Policy`.

### Example

A comment scores 0.95 on `hate_speech` and 0.5 on `toxicity`, against a rejection threshold of 0.8 and an escalation band of [0.3, 0.7]. Rejection takes precedence over the escalation-band score, and the comment is rejected without a reviewer seeing it.
