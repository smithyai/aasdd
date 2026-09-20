## Analysis Fails

A submission whose content cannot be analyzed ends in failure before any scoring.

> `Analyzing` — AnalysisError → `Failed`

- `AnalyzeContent` fails with `AnalysisError`.
- `ScoreContent`, `MakeDecision`, and `EscalateForReview` are never invoked.
- `ModerateContent` propagates `AnalysisError` to the caller and produces no outcome.
