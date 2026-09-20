## ScoringModel

### Context

`ScoreContent` computes a score per policy dimension from a submission and its features. The spec defines the dimensions and the score range but not what produces the scores.

### Requirement

A component that, given the submission and its `ContentFeatures`, returns a score in [0.0, 1.0] for each of `toxicity`, `spam`, and `hate_speech`, and that reports unavailability distinctly from a low score.

### Decision

A hosted text-classification service called over the network, one request per submission. A connection failure or an unsuccessful response is reported as `ScoringModelUnavailable`; a partial score set is treated the same way, never as a set of scores.
