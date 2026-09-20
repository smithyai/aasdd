The core methodology of Ability-Anchored Spec-Driven Development (AASDD): what it is, how specs are structured, how to author them correctly, and how they evolve over time.

## Overview

AASDD is a spec-driven methodology where systems are decomposed into **abilities** rather than components, services, or modules. An ability describes *what* a unit of the system can do — its purpose, inputs, outputs, behavioral invariants, and failure modes — without prescribing *how* it does it.

A spec is a self-contained unit containing a vision, ability definitions, shared concept types, scenarios, decisions, and optionally a state machine at the root ability level. A spec is written so that an implementation and its tests can be derived from it without asking the author a question: every behavior is a checkable condition, every open choice is recorded, and every scenario is an acceptance test. How a spec reaches that state, when it is complete enough to implement, and how the implementation run proceeds are defined in [PROCESS.md](PROCESS.md).

A spec may delegate a sub-ability to another spec. Beyond that, how multiple specs are organized or related — including how cross-spec type references are versioned when a dependency spec releases a breaking change — is intentionally left outside the scope of this methodology and left to the implementer.

## Spec Structure

```
spec.md             ← version, summary, vision, invariants
state-machine.md    ← orchestration FSM (optional)
abilities/          ← recursive ability decomposition
concepts/           ← shared type definitions
scenarios/          ← concrete execution traces
decisions/          ← choices the contracts leave open: transport, storage, model, behavior
```

Where the spec lives is left to the implementer. All that matters is that the directory contents conform to AASDD conventions. See [CONVENTIONS.md](CONVENTIONS.md) for naming rules and canonical file templates.

## Spec Anatomy

### Vision

`spec.md` carries the vision the decomposition serves, in three sections: **Purpose** (who the system is for and what problem it solves), **Non-Goals** (what it deliberately does not do), and **Success Criteria** (outcomes observable from outside the system, each tied to the root abilities that serve it and the scenarios that exercise it).

The vision is not a construct that competes with abilities. It is the root the tree grows from: root abilities are derived from success criteria, and a spec is judged complete against them. Non-goals bound the decomposition — an ability that serves a non-goal does not belong in the spec.

### Abilities

Every ability is a **black box**: typed inputs, typed outputs, invariants, and failure modes. The directory tree *is* the decomposition — nesting communicates parent-child relationships, and sub-abilities are internal to their parent.

A **root ability** is something the system does for someone outside it; its inputs cross the system boundary. Sub-abilities are internal: their inputs come from the parent or from sibling abilities.

Each ability is defined in an `ability.md` file. The first paragraph after the heading states what this ability does — one sentence. Then the required sections:

| Section           | Content                                                                   |
| ----------------- | ------------------------------------------------------------------------- |
| **Inputs**        | Typed inputs with descriptions                                            |
| **Outputs**       | Typed outputs with descriptions                                           |
| **Invariants**    | Behavioral rules that must always hold                                    |
| **Failure Modes** | Every way the ability can fail to produce its output, and how it responds |

Optional sections: `Idempotency` (present only when the ability guarantees that repeated invocation with identical inputs yields identical outputs, or has deduplication semantics worth specifying; absent means no guarantee is made) and `Composition` (absent on leaf abilities; required on non-leaf abilities once the spec reaches `1.0.0` — see below).

Abilities that produce effects still have typed outputs — the output is the acknowledgment of the effect, not the side effect itself.

Any spec file (`spec.md`, `ability.md`, `concept.md`, `scenario.md`, `state-machine.md`, `decision.md`) may include additional custom sections after all required and recognized optional sections. Custom sections carry no methodology semantics — the methodology does not define, interpret, or enforce them. Use custom sections for supplementary material such as figures, architecture diagrams, external references, or notes.

```
abilities/
  sanitize-workspace/
    ability.md
    identify-hazards/
      ability.md
    remove-hazards/
      ability.md
```

#### Composition

The tree shows which abilities a parent contains, not how data moves between them. A non-leaf ability states that in a **Composition** section: one step per sub-ability, in an order where every input is supplied by the parent or by an earlier step. Steps performed by the parent itself — assembling the output, choosing a branch — are marked with `—` in the Ability column.

Composition is what makes the implementation order mechanical: a sub-ability is implementable once its sources are. The root ability of a spec with a state machine omits Composition; the state machine's States and Transitions tables define its flow.

#### Delegation

A sub-ability may be **delegated** to another spec instead of being defined in place. A delegated `ability.md` contains only the heading, the purpose sentence, and the `**Spec:**` and `**Version:**` fields naming the delegated spec and the exact version the parent was authored against. The delegated spec must have exactly one root ability; that root ability's contract is the delegated ability's contract, and its inputs and outputs are referenced from the parent spec as external types.

The delegated spec's decisions bind its implementation. How the parent reaches it is a decision in the parent spec, and that decision must be compatible with the delegated spec's decisions.

Delegation is how a large product is specified as several specs of manageable size, each with its own version and its own readiness gate.

#### Placeholders

Two markers stand in for section content:

- `_None._` states that a section is intentionally empty. Inputs may be `_None._` when the ability's output is entirely implicit — derived from the tool's own built-in state with no caller-supplied parameters (e.g. listing the versions known to the tool). Failure Modes may be `_None._` only when no condition can prevent the ability from producing its output.
- `_Pending._` states that a required section has not been written yet. It is permitted only while the spec's version is below `1.0.0`, and it is how a spec stays conformant while it is decomposed one level at a time.

### Concepts

Shared types used across abilities, organized by domain. Every type in any ability's inputs or outputs must be defined here — except scalar types (`text`, `number`, `boolean`, `timestamp`), which are universally understood and need no definition, and types from another spec, which are referenced by name as plain text with a note indicating their external origin. Each `concept.md` file opens with a `## {DomainName} domain` heading that scopes all types within it.

A concept type is either a **structured type** with named properties or an **enum type** with a fixed set of values. Structured types list their properties in a Properties table; enum types list their values in a Value/Meaning table and have no properties.

```
concepts/
  workspace/
    concept.md       ← WorkspaceSnapshot, SanitizedWorkspace, …
  planning/
    concept.md       ← TaskPlan, ParsedIntent, …
```

### Scenarios

Concrete flows exercising the abilities in sequence. Each `scenario.md` contains one scenario: a description, a blockquote execution trace, and notable outcomes. Trace nodes are state names when the spec defines a state machine; use ability names when it does not. Every state name used in a trace must exist in the relevant `state-machine.md`, and every ability name must exist in the spec.

Scenarios are the acceptance suite of the spec, and the artifact a reviewer reacts to while the spec is being authored. At `1.0.0` they cover every success criterion, every failure mode of every root ability, and every transition of the state machine (Authoring Rules 15–17). An optional `### Example` section narrates a concrete instance in prose — this is where the experience of using the system is described in terms a reviewer can accept or reject.

### State Machine (optional)

A formal FSM for specs where abilities execute in a coordinated sequence with branching, retries, or interruption. Use when ordering constraints and transition legality must be formalized; skip when abilities are independently invocable.

A `state-machine.md` at the spec root coordinates the top-level lifecycle. A spec may define at most one state machine. If a sub-ability requires its own state-driven lifecycle, it is complex enough to be its own spec — and may be delegated to one. Each row of the Transitions table has exactly one trigger, so that scenario coverage of transitions is unambiguous.

### Decisions

A decision closes a choice that the contracts leave open. Some choices are technical: the transport for an input that crosses the system boundary, the storage mechanism for persistent state, the model or service powering a capability. Others are about behavior: whether a deletion is reversible, what a screen shows when there is nothing to show, which of two readings of a requirement is intended. Any choice that an implementer would otherwise have to make silently is recorded as a decision.

Every root-level ability input requires a transport decision — HTTP, gRPC, CLI, queue, or otherwise. The only exception is runtime-intrinsic values the environment provides for free, like the current timestamp, and abilities whose output is entirely implicit and require no inputs at all. Sub-ability inputs rarely need a decision because they are produced by the parent — a sub-ability almost always receives at least one parent-provided input. Depth is not the signal; what matters is whether the input arrives from outside the system.

A decision is either **open** or **closed**. An open decision records the Context, the Requirement, and the Options under consideration, with `_Open._` in place of the Decision. Open decisions are permitted only while the spec's version is below `1.0.0`; they are the enumerated list of what a reviewer must still settle. A closed decision is a binding constraint on the implementation — choosing gRPC eliminates languages and runtimes that can't support it. When a closed decision fixes behavior, that behavior is written into the invariants or failure modes of the abilities the decision names; the decision records *why*, the contract records *what*.

Each decision names, in its Context, every ability it constrains. All decisions in a spec must be mutually compatible. If two decisions point toward Python but a third rules it out, Python is off the table — the implementation must satisfy all decisions simultaneously. Incompatible decisions are a spec-level error and must be resolved before implementation begins.

`decisions/` lives at the spec root because a single decision often applies to multiple abilities — if all inputs arrive over HTTP, that's one decision, not one per ability.

## Authoring Rules

**Language agnosticism** — the spec never references a language, library, or framework. Use conceptual types. State invariants as rules. Describe failure modes as conditions.

**Decomposition:**

1. The directory hierarchy *is* the decomposition — there is no separate structural document.
2. Decompose when an ability's purpose spans more than one distinct responsibility.
3. Stop when an ability's behavior can be stated without ambiguity in a sentence or two.
4. Every non-leaf ability has a Composition section that names each sub-ability exactly once and the source of each of its inputs, in an order where every source is the parent or an earlier step. The root ability of a spec with a state machine is exempt.

**Completeness:**

5. Every ability must have typed outputs, invariants, and failure modes. Inputs are required unless the ability's output is entirely implicit, in which case the `### Inputs` section contains only `_None._`. Failure Modes may contain only `_None._` when no condition can prevent the ability from producing its output.
6. Every type must be defined in `concepts/`, unless it is a scalar type (`text`, `number`, `boolean`, `timestamp`) or a type from another spec referenced by name with a note indicating its external origin.
7. Cross-references use relative paths and must always resolve.
8. A required section may contain `_Pending._` in place of its content, and a decision may be `_Open._`, only while the spec's version is below `1.0.0`.

**Verifiability** — the standard for avoiding ambiguity:

9. Every invariant must be statable as a code-checkable condition. "The output is valid" is not an invariant. "The output list is non-empty" is.
10. Every failure mode condition must be precise enough to reproduce in a test. "Something goes wrong" is not a condition. "The input contains no resolvable dependencies" is.
11. Every success criterion must be an outcome observable from outside the system, stated so that a scenario can assert it. "The product is easy to use" is not a criterion. "A note captured with a title is retrievable by any word in that title" is.

**Traceability:**

12. Every root ability appears in the Abilities column of at least one success criterion, and every success criterion names at least one root ability.
13. Every decision names in its Context every ability it constrains.
14. When a decision that fixes behavior is closed, that behavior is added to the invariants or failure modes of the abilities it names.

**Coverage** — required at `1.0.0` and above:

15. Every success criterion names at least one scenario.
16. Every failure mode of every root ability appears as a divergence condition in at least one scenario.
17. When the spec defines a state machine, every transition appears in at least one scenario trace.

## Spec Versioning

Specs use **semantic versioning**. The version lives in `spec.md`.

| Change type | Example change                                                   | Version bump |
| ----------- | ---------------------------------------------------------------- | ------------ |
| **Major**   | Changed inputs/outputs, removed abilities, restructured concepts | `2.0.0`      |
| **Minor**   | New optional inputs, new abilities, new concept types            | `1.1.0`      |
| **Patch**   | Typos, wording, formatting                                       | `1.0.1`      |

Any spec with a version below `1.0.0` is in draft — concepts and abilities may still change freely, sections may be pending, decisions may be open, and implementations should not be treated as stable contracts. A spec reaches `1.0.0` by passing the readiness gate defined in [PROCESS.md](PROCESS.md#the-readiness-gate): no pending sections, no open decisions, the coverage rules satisfied, and tests derivable from the spec without a question. From `1.0.0` the contract is established and all subsequent changes follow semver strictly.

The `**AASDD:**` field in `spec.md` records which version of the AASDD methodology the spec was written against. AASDD uses simple integer versioning (`v1`, `v2`, …) — changes are infrequent and always breaking enough to warrant a full integer increment.

## Structured Representations

The file templates in [CONVENTIONS.md](CONVENTIONS.md) are fully deterministic: given a structured representation of a spec (abilities, concepts, states, transitions, etc.), every file can be generated mechanically with no judgment calls. The reverse is also true — a conforming spec directory can be parsed into a structured representation with no ambiguity. The conversion is lossless in both directions, and the padded table form defined in [CONVENTIONS.md](CONVENTIONS.md#formatting-rules) is the canonical rendering: rendering a spec from its structured form reproduces the markdown byte for byte.

This means a spec has two equivalent forms:

- **Markdown** — the directory of `.md` files defined by the templates in [CONVENTIONS.md](CONVENTIONS.md). This is the canonical form: human-readable, agent-consumable, and the artifact that is versioned and reviewed.
- **Structured** — a machine-readable representation (JSON, YAML, or similar) that captures the same information. Useful for tooling, programmatic access, diffing, and interchange between systems.

AASDD does not prescribe a schema for the structured form. As long as the conversion between structured and markdown is lossless, any schema that faithfully represents the spec constructs is valid.

Whatever the authoring workflow, the rendered markdown output must conform to the templates in [CONVENTIONS.md](CONVENTIONS.md).

For naming conventions, formatting rules, and canonical file templates, see [CONVENTIONS.md](CONVENTIONS.md). For the lifecycle of a spec — authoring, the readiness gate, and the implementation run — see [PROCESS.md](PROCESS.md). For translating specs into working code — ordering, testing, and the development cycle — see [IMPLEMENTATION.md](IMPLEMENTATION.md).
