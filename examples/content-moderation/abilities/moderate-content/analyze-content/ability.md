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
- `features.language` is set to a non-empty value.

### Failure Modes

| Failure         | Condition                                                                         | Effect                      |
| --------------- | --------------------------------------------------------------------------------- | --------------------------- |
| `AnalysisError` | The submission content is empty or cannot be parsed (e.g., unsupported encoding). | Error propagated to caller. |
