## ScoreContent

Computes policy dimension scores for a submission based on its extracted features.

### Inputs

| Name         | Type                                                                       | Description                             |
| ------------ | -------------------------------------------------------------------------- | --------------------------------------- |
| `submission` | [Submission](../../../concepts/submission/concept.md#submission)           | The submission being moderated.         |
| `features`   | [ContentFeatures](../../../concepts/submission/concept.md#contentfeatures) | Features extracted by `AnalyzeContent`. |

### Outputs

| Name     | Type                                                         | Description                                     |
| -------- | ------------------------------------------------------------ | ----------------------------------------------- |
| `scores` | [ScoreSet](../../../concepts/moderation/concept.md#scoreset) | Policy scores across all moderation dimensions. |

### Invariants

- Every score in `scores` is in the range [0.0, 1.0].

### Failure Modes

| Failure                   | Condition                            | Effect                      |
| ------------------------- | ------------------------------------ | --------------------------- |
| `ScoringModelUnavailable` | The scoring model cannot be reached. | Error propagated to caller. |
