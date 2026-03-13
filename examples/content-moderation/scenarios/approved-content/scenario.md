## Approved Content

**Description:** A submission with low scores on all dimensions is approved by policy without escalation.

> `Analyzing` → `Scoring` → `Deciding` → `Complete`

- `AnalyzeContent` extracts features without error.
- `ScoreContent` returns a `ScoreSet` with all scores below the rejection and escalation thresholds.
- `MakeDecision` returns `Approved`.
- Outcome has `decided_by` of `"policy"`.

### Example

A user posts a product review containing 42 tokens in English with no flagged patterns. All three scores are below 0.2. `MakeDecision` returns `Approved` and the pipeline completes immediately.