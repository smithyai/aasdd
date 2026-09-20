## Scoring Model Unavailable

A submission is analyzed, but the scoring model cannot be reached, so processing fails without a decision.

> `Analyzing` → `Scoring` — ScoringModelUnavailable → `Failed`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` fails with `ScoringModelUnavailable`.
- `MakeDecision` and `EscalateForReview` are never invoked.
- `ModerateContent` propagates `ScoringModelUnavailable` to the caller and produces no outcome.
