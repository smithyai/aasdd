## DocumentTransport

### Context

`CheckDocument` accepts a `Document` as its root-level input. That document must arrive from outside the system, which requires choosing a transport mechanism.

### Requirement

The implementation must provide a way for a caller to submit a `Document` and receive back either a `DocumentReport` or the `UnparsableDocument` error.

### Decision

HTTP — the caller submits the document as the request body and receives the report in the response body. An `UnparsableDocument` failure is returned as a client error response.
