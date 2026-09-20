## PolicySource

### Context

`ModerateContent` takes a `ModerationPolicy` as a root-level input alongside the submission. The policy is operational configuration owned by the platform, not something each caller supplies, so it needs a source of its own.

### Requirement

Every invocation of `ModerateContent` must receive the policy currently in force, and the same policy must apply for the whole of one submission's processing.

### Decision

The policy is read from a configuration file at process start and held in memory; changing it requires a restart. The service supplies the loaded policy when it invokes `ModerateContent`, so the `InteractionChannel` RPC carries only the `Submission`.
