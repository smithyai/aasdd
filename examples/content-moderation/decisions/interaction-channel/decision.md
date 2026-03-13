## InteractionChannel

### Context

`ModerateContent` accepts a `Submission` as its top-level input. The spec does not prescribe how a `Submission` is acquired — it only defines the type. The implementation must choose a concrete inbound transport and establish an abstract interface for it before `ModerateContent` can be wired up end-to-end.

### Requirement

A mechanism to receive a `Submission` from an external caller and deliver it to `ModerateContent`, returning the resulting `ModerationOutcome` to that caller.

### Decision

Expose a gRPC service. The caller invokes a unary RPC with a `Submission` message; the service invokes `ModerateContent` and returns the `ModerationOutcome` as the response message. The transport layer is defined as an abstract interface so the core ability is testable without a running gRPC server.