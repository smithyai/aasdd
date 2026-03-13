## Submission domain

Types representing user-generated content submitted for moderation.

### Submission

A piece of user-generated content awaiting moderation review.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `id` | text | Unique identifier for the submission. |
| `content` | text | The raw content to be moderated. |
| `submitted_at` | timestamp | When the submission was received. |

### ContentFeatures

Textual and structural features extracted from a submission.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `language` | text | Detected language code (e.g., `"en"`, `"es"`). |
| `token_count` | number | Number of tokens in the content. |
| `flagged_patterns` | list of text | Patterns matched against a known-problematic pattern list. |