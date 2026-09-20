## Approved Content

A submission with low scores on all dimensions is approved by policy without escalation.

> `Analyzing` → `Scoring` → `Deciding` → `Complete`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` returns a `ScoreSet` with every score below `policy.escalation_band.lower`.
- `MakeDecision` returns `Approved`.
- The outcome has `verdict` of `Approved` and `decided_by` of `Policy`.

### Example

A user posts a product review containing 42 tokens in English with no flagged patterns. All three scores are below 0.2, under an escalation band of [0.3, 0.7]. `MakeDecision` returns `Approved` and the pipeline completes immediately.
