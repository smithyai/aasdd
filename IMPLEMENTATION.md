This document describes the suggested approach for translating an AASDD spec into working code. It is not the only valid path — teams adapt the ordering, tooling, and workflow to their context. What doesn't change is the spec's authority: implementation follows from it. For broader implementation strategy — including how to implement abilities in isolation and connect them later — see [From Spec to Implementation](https://github.com/smithyai/aasdd/wiki/From-Spec-to-Implementation).

Agents are the primary intended use case. Because AASDD specs are precise and unambiguous — typed inputs, typed outputs, code-checkable invariants, reproducible failure modes — they are an ideal operating substrate for agents. The [Agent instructions](#agent-instructions) section below provides a ready-to-copy block for any implementation repository.

## Order

1. Establish contracts before the ability that uses them — concept types, ability entry-point signatures, and any infrastructure interfaces derived from `decisions/`. These are build prerequisites; when writing code for an ability, its contracts must already exist.
2. Implement abilities with no upstream ability dependencies first.
3. Work outward through the dependency graph, implementing each ability once all abilities that produce its inputs are complete.

## Testing

| Spec construct   | Test obligation                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------- |
| **Invariant**    | Assert the condition holds on every valid output                                          |
| **Failure mode** | One test per failure mode that triggers the exact condition and verifies the effect       |
| **Scenario**     | One integration test per scenario, asserting the full execution trace and outcomes        |
| **Idempotency**  | A test that invokes the ability twice with identical inputs and asserts identical outputs |

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

| Spec construct   | Implementation                                                                                                                   |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Concept type** | A concrete type in the language (struct, class, record, etc.) — use the exact PascalCase name from the spec                      |
| **Ability**      | A module or function — the entry point uses the snake_case form of the ability name                                              |
| **Invariant**    | An assertion — use the language's assertion or guard mechanism for cheap checks; return an error for runtime-enforced invariants |
| **Failure mode** | An error variant — the PascalCase name from the spec maps directly to the variant name                                           |

Concept names are canonical. Never rename a type or ability in the implementation — adapt casing to the language convention (`WorkspaceSnapshot` → `workspace_snapshot` in snake_case), but keep the name itself identical.

If a failure path exists in the implementation but has no corresponding spec failure mode, the spec is incomplete — add the failure mode before merging. If new behavior, inputs, outputs, or options are added that are not reflected in the spec, update the spec first — implementation follows the spec, not the other way around.

## Agent instructions

The block below is designed to be copied into any implementation repository as `AGENTS.md`, `copilot-instructions.md`, or equivalent, with minimal adaptation. It is self-contained and language-agnostic.

````markdown
This repository uses **Ability-Anchored Spec-Driven Development (AASDD)**. Read the spec before making any changes — it is the source of truth. The full methodology is at [aasdd](https://github.com/smithyai/aasdd).

## The spec

The spec structure, anatomy, and authoring rules are defined in [METHODOLOGY.md](https://github.com/smithyai/aasdd/blob/main/METHODOLOGY.md). Before implementing any ability, read its `ability.md` and every `concept.md` file it references. If a `decisions/` folder exists, read it before starting any implementation.

If the spec version is below `1.0.0`, the contract may shift. Do not make irreversible implementation decisions against it unless you accept that risk. At `1.0.0` and above the contract is established — implementation tracks the spec version.

## Translation rules

| Spec construct | Implementation                                                                                                                   |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Concept type   | A concrete type in the language (struct, class, record, etc.) — use the exact PascalCase name from the spec                      |
| Ability        | A module or function — the entry point uses the snake_case form of the ability name                                              |
| Invariant      | An assertion — use the language's assertion or guard mechanism for cheap checks; return an error for runtime-enforced invariants |
| Failure mode   | An error variant — the PascalCase name from the spec maps directly to the variant name                                           |

Concept names are canonical. Never rename a type or ability in the implementation — adapt casing to the language convention (`WorkspaceSnapshot` → `workspace_snapshot` in snake_case), but keep the name itself identical.

## Implementation

Follow [IMPLEMENTATION.md](https://github.com/smithyai/aasdd/blob/main/IMPLEMENTATION.md) for ordering rules, testing obligations, and the development cycle.

Key reminders:
- If a failure path exists in the implementation but has no corresponding spec failure mode, the spec is incomplete — add the failure mode before merging.
- If new behavior, inputs, outputs, or options are added that are not reflected in the spec (abilities, concepts, or decisions), update the spec first — implementation follows the spec, not the other way around.
- If the spec includes a state machine, implement transitions exactly as defined in the Transitions table. A sub-ability may have its own `state-machine.md` scoped to its directory — treat it the same way, constrained to that ability's implementation.
- Never change implementation to match test expectations that contradict the spec.
````
