## InteractionChannel

### Context

`ModerateContent` accepts a `Submission` as a root-level input. The spec does not prescribe how a `Submission` is acquired — it only defines the type. The implementation must choose a concrete inbound transport and establish an abstract interface for it before `ModerateContent` can be wired up end-to-end. The `policy` input is not part of this channel; see `PolicySource`.

### Requirement

A mechanism to receive a `Submission` from an external caller and deliver it to `ModerateContent`, returning the resulting `ModerationOutcome`, or the failure that ended processing, to that caller.

### Decision

Expose a gRPC service. The caller invokes a unary RPC with a `Submission` message; the service invokes `ModerateContent` with that submission and the policy in force, and returns the `ModerationOutcome` as the response message. A failure is returned as an RPC error named after the failure mode. The transport layer is defined as an abstract interface so the core ability is testable without a running gRPC server.
