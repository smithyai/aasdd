## Invalid Policy

A submission is analyzed and scored, but the policy in force is inconsistent, so no decision can be made.

> `Analyzing` → `Scoring` → `Deciding` — InvalidPolicy → `Failed`

- `AnalyzeContent` and `ScoreContent` complete without error.
- `MakeDecision` fails with `InvalidPolicy` because `policy.escalation_band.upper` is not below `policy.rejection_threshold`.
- `EscalateForReview` is never invoked.
- `ModerateContent` propagates `InvalidPolicy` to the caller and produces no outcome.
