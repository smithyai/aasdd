## Content Moderation

**AASDD:** v2
**Version:** 1.0.0

Accepts a user-generated submission and produces a moderation outcome by analyzing content, scoring it across policy dimensions, and applying a decision — escalating to human review when scores fall in an ambiguous range.

### Purpose

For platforms that accept user-generated text and must keep clearly harmful content off the platform without sending every submission to a human. It automates the clear cases and routes only the ambiguous ones to reviewers, so that reviewer time is spent where judgment is needed.

### Non-Goals

- Does not define moderation policy: thresholds, bands, and deadlines are supplied to the system, not chosen by it.
- Does not moderate images, audio, or video.
- Does not notify submitters of outcomes or offer appeals.

### Success Criteria

| Criterion                                                                                     | Abilities         | Scenarios                                                                                |
| --------------------------------------------------------------------------------------------- | ----------------- | ---------------------------------------------------------------------------------------- |
| Clearly acceptable content is approved automatically, without human involvement.              | `ModerateContent` | `approved-content`                                                                       |
| Clearly harmful content is rejected automatically.                                            | `ModerateContent` | `rejected-content`                                                                       |
| Ambiguous content reaches a human reviewer, whose verdict is final.                           | `ModerateContent` | `escalated-and-reviewed`                                                                 |
| No submission waits on a reviewer beyond the policy's deadline.                               | `ModerateContent` | `review-timed-out`                                                                       |
| A submission that cannot be moderated is reported as a failure rather than silently approved. | `ModerateContent` | `analysis-fails`, `scoring-model-unavailable`, `invalid-policy`, `no-reviewer-available` |

### Invariants

- Every submission that enters the pipeline receives exactly one `ModerationOutcome` or exactly one failure.
- A submission is never automatically approved or rejected while it is under human review.
