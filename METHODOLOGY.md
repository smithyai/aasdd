The core methodology of Ability-Anchored Spec-Driven Development (AASDD): what it is, how specs are structured, how to author them correctly, and how they evolve over time.

## Overview

AASDD is a spec-driven methodology where systems are decomposed into **abilities** rather than components, services, or modules. An ability describes *what* a unit of the system can do — its purpose, inputs, outputs, behavioral invariants, and failure modes — without prescribing *how* it does it.

A spec is a self-contained unit containing ability definitions, shared concept types, and optionally: scenarios, state machines (one per directory that owns transitions, each scoped to that directory), and technology decisions. How multiple specs are organized or related — including how cross-spec type references are versioned when a dependency spec releases a breaking change — is intentionally left outside the scope of this methodology and left to the implementer.

## Spec Structure

```
spec.md             ← version, purpose, invariants
state-machine.md    ← orchestration FSM (optional)
abilities/          ← recursive ability decomposition
concepts/           ← shared type definitions
scenarios/          ← concrete execution traces (optional)
decisions/          ← technology decisions: transport, storage, model (required once root-level inputs exist)
```

Where the spec lives is left to the implementer. All that matters is that the directory contents conform to AASDD conventions. See [CONVENTIONS.md](CONVENTIONS.md) for naming rules and canonical file templates.

## Spec Anatomy

### Abilities

Every ability is a **black box**: typed inputs, typed outputs, invariants, and failure modes. The directory tree *is* the decomposition — nesting communicates parent-child relationships, and sub-abilities are internal to their parent.

Each ability is defined in an `ability.md` file with these required sections:

| Section | Purpose |
| --- | --- |
| **Purpose** | One sentence: what this ability does |
| **Inputs** | Typed inputs with descriptions |
| **Outputs** | Typed outputs with descriptions |
| **Invariants** | Behavioral rules that must always hold |
| **Failure Modes** | Exhaustive: what can go wrong and how it responds |

Optional sections: `Idempotency` (present when the ability has retry or deduplication semantics worth specifying) and `Visualization` (a diagram of sub-ability structure, present only when the ability has sub-ability subdirectories). Visualization is for structure, not explanation — omit it whenever the directory tree and prose are sufficient.

Abilities that produce effects still have typed outputs — the output is the acknowledgment of the effect, not the side effect itself.

```
abilities/
  sanitize-workspace/
    ability.md
    identify-hazards/
      ability.md
    remove-hazards/
      ability.md
```

### Concepts

Shared types used across abilities, organized by domain. Every type in any ability's inputs or outputs must be defined here — except scalar types (`text`, `number`, `boolean`, `timestamp`), which are universally understood and need no definition, and types from another spec, which are referenced by name as plain text with a note indicating their external origin. Each `concept.md` file opens with a `## {DomainName} domain` heading that scopes all types within it.

```
concepts/
  workspace/
    concept.md       ← WorkspaceSnapshot, SanitizedWorkspace, …
  planning/
    concept.md       ← TaskPlan, ParsedIntent, …
```

### Scenarios (optional)

Concrete flows exercising the abilities in sequence. Illustrative, not exhaustive — useful for clarifying composition and defining integration test obligations. Each `scenario.md` contains one scenario: a description, a blockquote execution trace, and notable outcomes. Trace nodes are state names when the spec defines a state machine; use ability names when it does not.

### State Machine (optional)

A formal FSM for specs where abilities execute in a coordinated sequence with branching, retries, or interruption. Use when ordering constraints and transition legality must be formalized; skip when abilities are independently invocable.

A `state-machine.md` at the spec root coordinates the top-level lifecycle. A sub-ability that requires its own internal orchestration may have a `state-machine.md` scoped to its own directory — it follows the same template and is invisible to anything outside that ability.

Place `state-machine.md` in the directory of the ability that owns its transitions — scope it no higher than necessary.

### Decisions

Every root-level ability input requires a transport decision — HTTP, gRPC, CLI, queue, or otherwise. The only exception is runtime-intrinsic values the environment provides for free, like the current timestamp, and abilities whose output is entirely implicit and require no inputs at all. Sub-ability inputs rarely need a decision because they are produced by the parent — a sub-ability almost always receives at least one parent-provided input. Any additional caller-supplied inputs to a sub-ability (configuration, thresholds, credentials) are secondary and almost never cross a system boundary. Depth is not the signal; what matters is whether the input arrives from outside the system.

The spec leaves the choice open; the decision closes it. Once recorded, a decision is a binding constraint on the implementation — choosing gRPC eliminates languages and runtimes that can't support it. What's optional is whether a decision exists yet; a spec can be valid without one if the choice hasn't been made. Each `decision.md` has Context, Requirement, and Decision sections.

All decisions in a spec must be mutually compatible. If two decisions point toward Python but a third rules it out, Python is off the table — the implementation must satisfy all decisions simultaneously. Incompatible decisions are a spec-level error and must be resolved before implementation begins.

`decisions/` lives at the spec root because a single decision often applies to multiple root-level abilities — if all inputs arrive over HTTP, that's one decision, not one per ability.

## Authoring Rules

**Language agnosticism** — the spec never references a language, library, or framework. Use conceptual types. State invariants as rules. Describe failure modes as conditions.

**Decomposition:**

1. The directory hierarchy *is* the decomposition — there is no separate structural document.
2. Decompose when an ability's purpose spans more than one distinct responsibility.
3. Stop when an ability's behavior can be stated without ambiguity in a sentence or two.

**Completeness:**

4. Every ability must have typed outputs, invariants, and failure modes. Inputs are required unless the ability's output is entirely implicit — derived from the tool's own built-in state with no caller-supplied parameters (e.g. listing the versions known to the tool). When inputs are absent, the `### Inputs` section must be present and contain only `_None._` to make the absence explicit.
5. Every type must be defined in `concepts/`, unless it is a scalar type (`text`, `number`, `boolean`, `timestamp`) or a type from another spec referenced by name with a note indicating its external origin.
6. Cross-references use relative paths and must always resolve.

**Verifiability** — the standard for avoiding ambiguity:

7. Every invariant must be statable as a code-checkable condition. "The output is valid" is not an invariant. "The output list is non-empty" is.
8. Every failure mode condition must be precise enough to reproduce in a test. "Something goes wrong" is not a condition. "The input contains no resolvable dependencies" is.

## Spec Versioning

Specs use **semantic versioning**. The version lives in `spec.md`.

| Change type | Example change | Version bump |
| --- | --- | --- |
| **Major** | Changed inputs/outputs, removed abilities, restructured concepts | `v2.0.0` |
| **Minor** | New optional inputs, new abilities, new concept types | `v1.1.0` |
| **Patch** | Typos, wording, formatting | `v1.0.1` |

**Status** signals spec maturity:

- **Draft** — Actively being designed. Concepts and abilities may still change freely. Do not implement against a Draft spec unless you accept that the contract may shift under you.
- **Stable** — The contract is established. Changes follow semver strictly. Implementation tracks the spec version.
- **Deprecated** — No new abilities will be added.

Implementations should pin to a specific spec version to record which contract they target.

The `**AASDD:**` field in `spec.md` records which version of the AASDD methodology the spec was written against. AASDD uses simple integer versioning (`v1`, `v2`, …) — changes are infrequent and always breaking enough to warrant a full integer increment. Tooling can use this field to select the correct rule set when verifying or scaffolding a spec.

## Structured Authoring

The file templates in [CONVENTIONS.md](CONVENTIONS.md) are fully deterministic: given a structured representation of a spec (abilities, concepts, states, transitions, etc.), every file can be generated mechanically with no judgment calls. An implementer may prefer to author a spec as structured data — JSON, YAML, or similar — and render the markdown files from it.

AASDD does not prescribe a schema or toolchain for this. The markdown files remain the canonical artifact — for a deliberate reason.

Agents consume specs directly. A well-structured markdown document — headings, prose, tables — is not a limitation compared to JSON; it is an advantage. Agents parse and reason over natural language far more reliably than they navigate deeply nested objects with opaque keys. A spec written in prose with clear section structure gives an agent the same information as a JSON schema, plus the semantic context that makes it unambiguous. The formatting conventions in [CONVENTIONS.md](CONVENTIONS.md) exist precisely to make specs both human-readable and agent-consumable without sacrificing either.

Whatever the authoring workflow, the rendered output must conform to the templates in [CONVENTIONS.md](CONVENTIONS.md).

For naming conventions, formatting rules, and canonical file templates, see [CONVENTIONS.md](CONVENTIONS.md). For translating specs into working code — ordering, testing, and the development cycle — see [IMPLEMENTATION.md](IMPLEMENTATION.md).
