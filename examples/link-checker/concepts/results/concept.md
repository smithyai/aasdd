## Results domain

Types representing the outcome of link verification.

### LinkStatus

The outcome of verifying a single link.

| Value | Meaning |
| --- | --- |
| `Alive` | The link responded successfully. |
| `Dead` | The link returned an error response. |
| `Unreachable` | The host could not be contacted. |

### LinkResult

The verification result for a single link.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `link` | [Link](../document/concept.md#link) | The link that was verified. |
| `status` | [LinkStatus](#linkstatus) | The outcome of verifying the link. |

### DocumentReport

A summary of all link verification results for a document.

#### Properties

| Name | Type | Description |
| --- | --- | --- |
| `results` | list of [LinkResult](#linkresult) | The individual result for each link. |
| `total` | number | Total number of links checked. |
| `alive` | number | Number of links with status `Alive`. |
| `dead` | number | Number of links with status `Dead`. |
| `unreachable` | number | Number of links with status `Unreachable`. |
