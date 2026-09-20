## AnalyzeContent

Parses a submission and extracts textual features needed for scoring.

### Inputs

| Name         | Type                                                             | Description                |
| ------------ | ---------------------------------------------------------------- | -------------------------- |
| `submission` | [Submission](../../../concepts/submission/concept.md#submission) | The submission to analyze. |

### Outputs

| Name       | Type                                                                       | Description                            |
| ---------- | -------------------------------------------------------------------------- | -------------------------------------- |
| `features` | [ContentFeatures](../../../concepts/submission/concept.md#contentfeatures) | Extracted features for the submission. |

### Invariants

- `features.token_count` is greater than zero.
- `features.language` is a non-empty language code.
- Every entry in `features.flagged_patterns` occurs in `submission.content`.

### Failure Modes

| Failure         | Condition                                                    | Effect                      |
| --------------- | ------------------------------------------------------------ | --------------------------- |
| `AnalysisError` | `submission.content` is empty, or cannot be decoded as text. | Error propagated to caller. |
