This document describes the suggested approach for translating an AASDD spec into working code. It is not the only valid path — teams adapt the ordering, tooling, and workflow to their context. What doesn't change is the spec's authority: implementation follows from it. The lifecycle around implementation — authoring, the readiness gate, the implementation run, and closing — is defined in [PROCESS.md](PROCESS.md). For broader implementation strategy — including how to implement abilities in isolation and connect them later — see [From Spec to Implementation](https://github.com/smithyai/aasdd/wiki/From-Spec-to-Implementation).

Agents are the primary intended use case. Because AASDD specs are precise and unambiguous — typed inputs, typed outputs, code-checkable invariants, reproducible failure modes, scenarios that double as acceptance tests — they are an ideal operating substrate for agents. The [Agent instructions](#agent-instructions) section below provides a ready-to-copy block for any implementation repository.

## Order

1. Establish contracts before the ability that uses them — concept types, ability entry-point signatures, and any infrastructure interfaces derived from `decisions/`. These are build prerequisites; when writing code for an ability, its contracts must already exist.
2. Implement abilities with no upstream ability dependencies first. A sub-ability's dependencies are the sources named in its parent's Composition section, or the transitions that feed its state in the state machine.
3. Work outward through the dependency graph, implementing each ability once all abilities that produce its inputs are complete. A delegated ability is a dependency on the delegated spec's implementation, not something to implement locally.

## Testing

| Spec construct        | Test obligation                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------- |
| **Invariant**         | Assert the condition holds on every valid output                                          |
| **Failure mode**      | One test per failure mode that triggers the exact condition and verifies the effect       |
| **Scenario**          | One integration test per scenario, asserting the full execution trace and outcomes        |
| **Success criterion** | Covered by the integration tests of the scenarios it names; no separate test              |
| **Idempotency**       | A test that invokes the ability twice with identical inputs and asserts identical outputs |

Composition sections and decisions carry no test obligation of their own: Composition determines order, and a decision's consequences are tested through the invariants and failure modes it produced.

Tests are derived before implementation. Deriving them is also the last readiness condition in [PROCESS.md](PROCESS.md#the-readiness-gate): a spec from which every test can be written without a question is ready.

## Development cycle

```
        ┌──────────────────────────────────────────┐
        ↓                                          │
spec → contracts → tests → implementation          │
                                 │                 │
                           gap discovered          │
                                 └── update spec ──┘
```

*Contracts: concept types, ability entry-point signatures, and infrastructure interfaces derived from `decisions/`.*

## Translation rules

| Spec construct        | Implementation                                                                                                                   |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Concept type**      | A concrete type in the language (struct, class, record, etc.) — use the exact PascalCase name from the spec                      |
| **Ability**           | A module or function — the entry point uses the snake_case form of the ability name                                              |
| **Delegated ability** | A call into the delegated spec's implementation, reached as the parent spec's decision for it requires — no local implementation |
| **Invariant**         | An assertion — use the language's assertion or guard mechanism for cheap checks; return an error for runtime-enforced invariants |
| **Failure mode**      | An error variant — the PascalCase name from the spec maps directly to the variant name                                           |

Concept names are canonical. Never rename a type or ability in the implementation — adapt casing to the language convention (`WorkspaceSnapshot` → `workspace_snapshot` in snake_case), but keep the name itself identical.

If a failure path exists in the implementation but has no corresponding spec failure mode, the spec is incomplete — add the failure mode before merging. If new behavior, inputs, outputs, or options are added that are not reflected in the spec, update the spec first — implementation follows the spec, not the other way around. If the spec does not answer a question the implementation needs answered, do not guess: record an open decision as [PROCESS.md](PROCESS.md#the-implementation-run) describes.

## Agent instructions

The block below is designed to be copied into any implementation repository as `AGENTS.md`, `copilot-instructions.md`, or equivalent, with minimal adaptation. It is self-contained and language-agnostic.

````markdown
This repository uses **Ability-Anchored Spec-Driven Development (AASDD)**. Read the spec before making any changes — it is the source of truth. The full methodology is at [aasdd](https://github.com/smithyai/aasdd).

## The spec

The spec structure, anatomy, and authoring rules are defined in [METHODOLOGY.md](https://github.com/smithyai/aasdd/blob/main/METHODOLOGY.md). Read `spec.md` first: its Purpose, Non-Goals, and Success Criteria are the intent behind every ability. Before implementing any ability, read its `ability.md`, every `concept.md` it references, and every decision in `decisions/` whose Context names it.

If the spec version is below `1.0.0`, it has not passed the readiness gate: sections may be `_Pending._` and decisions may be `_Open._`. Do not make irreversible implementation decisions against it unless you accept that the contract may shift. At `1.0.0` and above the contract is established — implementation tracks the spec version.

## Translation rules

| Spec construct    | Implementation                                                                                                                   |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Concept type      | A concrete type in the language (struct, class, record, etc.) — use the exact PascalCase name from the spec                      |
| Ability           | A module or function — the entry point uses the snake_case form of the ability name                                              |
| Delegated ability | A call into the delegated spec's implementation, reached as the parent spec's decision for it requires — no local implementation |
| Invariant         | An assertion — use the language's assertion or guard mechanism for cheap checks; return an error for runtime-enforced invariants |
| Failure mode      | An error variant — the PascalCase name from the spec maps directly to the variant name                                           |

Concept names are canonical. Never rename a type or ability in the implementation — adapt casing to the language convention (`WorkspaceSnapshot` → `workspace_snapshot` in snake_case), but keep the name itself identical.

## Implementation

Follow [IMPLEMENTATION.md](https://github.com/smithyai/aasdd/blob/main/IMPLEMENTATION.md) for ordering rules, testing obligations, and the development cycle, and [PROCESS.md](https://github.com/smithyai/aasdd/blob/main/PROCESS.md) for how an implementation run proceeds.

Key reminders:
- After any change to a spec file, run `aasdd verify <spec-dir>` to confirm the spec conforms to the methodology and conventions. Fix all reported violations before proceeding.
- Derive tests from the spec before writing implementation: one per invariant, one per failure mode, one per scenario, and one for each Idempotency section.
- Implement sub-abilities in the order their parent's Composition table defines. If the spec includes a state machine, implement transitions exactly as defined in the Transitions table.
- If a failure path exists in the implementation but has no corresponding spec failure mode, the spec is incomplete — add the failure mode before merging.
- If the spec does not answer a question the implementation needs answered, do not guess: add an open decision under `decisions/` whose Context names the ability, skip that ability and everything that depends on it, and continue with the rest.
- If new behavior, inputs, outputs, or options are added that are not reflected in the spec (abilities, concepts, or decisions), update the spec first — implementation follows the spec, not the other way around.
- Never change implementation to match test expectations that contradict the spec.
````
