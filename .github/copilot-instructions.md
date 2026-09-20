This repository is the source definition of the **AASDD methodology**. It is an open-source project — anyone building software with AASDD derives their specs, tooling, and agent workflows from what is written here. Every change has downstream consequences. Treat this the way you would treat a standard or a language specification: with precision, restraint, and awareness of who depends on the output.

There is no code here. No tests to run, no application to ship. The artifacts are prose documents, canonical file templates, and reference examples. Quality means correctness, internal consistency, and stability — not features shipped.

## Core principle: ability-anchored

The single architectural decision this methodology rests on: **abilities are the central organizing unit**. Every construct, rule, template, and example exists to support ability-driven decomposition. This is not a stylistic preference. It is the load-bearing design choice. Any change that weakens, dilutes, or works around it is a regression regardless of what it adds.

How this constrains development of the methodology itself:

- **No construct may exist without tracing to abilities.** Concepts define types that abilities consume and produce. Decisions bind choices that abilities leave open. Scenarios trace ability composition. State machines coordinate ability execution. The vision in `spec.md` is the root the ability tree grows from: success criteria exist to be served by root abilities and exercised by scenarios. If a proposed new construct — a new file type, a new template section, a new rule — cannot be justified by its relationship to at least one ability, it does not belong in the methodology.
- **No construct may be elevated to parity with abilities.** Concepts are not peers — they are supporting types. Decisions are not peers — they are binding choices. The vision is not a peer — it is the input to decomposition. If a change gives any other construct its own decomposition hierarchy, its own versioning story, or its own independent lifecycle, it undermines the anchor.
- **Decomposition is the directory tree, period.** Nesting directories communicates parent-child ability relationships. There is no registry, no manifest, no architectural overview document. A parent's Composition section describes data flow among its own children and nothing else; it is not a structural document. If a change introduces a parallel structure for describing how abilities relate to each other across the tree — an index file, a dependency graph document, a relationship table — it conflicts with this principle.
- **The term "ability" is load-bearing.** It was chosen because it carries no implementation baggage (unlike "component," "service," or "module"). Do not introduce synonyms, aliases, or shorthands anywhere in the methodology documents. The term is the anchor.

When evaluating any proposed change: does this keep abilities as the single point that every other construct orbits? If the answer is ambiguous, the change needs rethinking.

## What lives where

The methodology is defined across four documents and demonstrated by three examples. Each has a specific role and strict boundaries.

| Artifact                          | Role                                                                             | Boundaries                                                                                                                      |
| --------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `METHODOLOGY.md`                  | Spec anatomy, authoring rules, versioning                                        | Defines *what* AASDD is. Never prescribes languages, frameworks, or tooling. May use them as illustrative examples.             |
| `CONVENTIONS.md`                  | Naming rules, formatting rules, canonical file templates                         | Defines *how* spec files are structured. Templates must be deterministic and parseable.                                         |
| `PROCESS.md`                      | Spec lifecycle: authoring procedure, readiness gate, implementation run, closing | Defines *when* a spec is complete and *how* it becomes code without human review. Rules only; workflow tips belong in the wiki. |
| `IMPLEMENTATION.md`               | Translation rules, testing obligations, development cycle, agent instructions    | Defines *how* specs become code. Language-agnostic. Contains the downstream-consumed agent instructions block.                  |
| `README.md`                       | Entry point                                                                      | Links to documents, examples, tooling, and wiki, and summarizes what changed in the current version. No rules defined here.     |
| `examples/`                       | Reference specs                                                                  | Three specs that together exercise every construct. Must stay conformant when rules change.                                     |
| `LICENSE`                         | License                                                                          | Project license. Not part of the methodology.                                                                                   |
| `_config.yml`                     | GitHub Pages                                                                     | Jekyll configuration for GitHub Pages presentation. Not part of the methodology.                                                |
| `.gitattributes`, `.editorconfig` | Line endings and whitespace                                                      | Enforce the LF and final-newline rules in `CONVENTIONS.md` at checkout and in editors. Not part of the methodology.             |
| `.github/workflows/`              | CI                                                                               | Verifies the examples with `aasdd-cli` and publishes releases on tags. Not part of the methodology.                             |
| `.gitignore`                      | Git ignore                                                                       | Prevents OS artifacts (e.g. `.DS_Store`) from being committed. Not part of the methodology.                                     |

These four documents form a closed system. Every rule the methodology enforces must be traceable to exactly one of them. If a rule cannot be placed in any of the four, it either belongs in the [wiki](https://github.com/smithyai/aasdd/wiki) as guidance or does not belong at all.

The wiki is not part of the methodology definition — it provides practical guidance, tips, and worked examples. Rules live in the core documents. Guidance lives in the wiki. Do not blur this boundary.

## Developing the core documents

### Cross-document consistency

A change to one document almost always requires changes to the others. The four documents share terminology, reference each other, and collectively define the rule set. Verify consistency across all four before finalizing any change:

- A new spec construct in `METHODOLOGY.md` needs a template in `CONVENTIONS.md`, may need a test obligation row in `IMPLEMENTATION.md`, and may need a readiness condition in `PROCESS.md`.
- A new template in `CONVENTIONS.md` must correspond to a construct defined in `METHODOLOGY.md`.
- A new translation rule in `IMPLEMENTATION.md` must map to constructs that exist in the other documents.
- A readiness condition in `PROCESS.md` must reference the authoring rule in `METHODOLOGY.md` it enforces, or be structural enough to check without one.
- `PROCESS.md` refers to authoring rules by number. Renumbering a rule means updating every reference.

Do not assume a change to one document is self-contained. Read the relevant sections of all four.

### Writing rules vs. writing guidance

The authoring rules in `METHODOLOGY.md` and the readiness conditions in `PROCESS.md` are hard constraints — every compliant spec must satisfy them. When adding or modifying rules:

- **State rules as verifiable conditions.** "Every invariant must be statable as a code-checkable condition" is a rule. "Invariants should be clear" is not. If you cannot state a rule as a condition that an agent or tool could check without human judgment, it is guidance, not a rule, and belongs in the wiki.
- **Rules must be enforceable.** A rule that requires subjective evaluation ("the spec should be well-organized") cannot be enforced and does not belong in the methodology documents.

### Maintaining template determinism

The file templates in `CONVENTIONS.md` define a lossless bidirectional mapping between markdown specs and structured representations (JSON, YAML). [aasdd-cli](https://github.com/smithyai/aasdd-cli) and other tooling depend on this property — parsing a spec to structured form and rendering it back must produce identical output. The examples are kept in the canonical padded table form so that this holds for them byte for byte. When modifying templates:

- **Every section must be unambiguously parseable.** If a section's presence or content depends on context outside the file, the lossless property breaks.
- **Section order is fixed.** Parsers and agents rely on positional structure. Do not make section order flexible.
- **Required vs. optional must be explicit.** Optional sections are "omitted entirely when not applicable" — partially present optional sections break parsing. The placeholders `_None._`, `_Pending._`, and `_Open._` are the only permitted stand-ins for section content, and each has a defined meaning.

### The agent instructions block is a public API

`IMPLEMENTATION.md` contains an agent instructions block designed to be copied verbatim into implementation projects as `copilot-instructions.md`, `AGENTS.md`, or equivalent. Every project that has copied this block is a downstream consumer. Changes propagate to all of them on next adoption. Treat this block with the same discipline as a public API:

- **Inline essentials, references for depth.** The block includes translation rules and key reminders inline. It references METHODOLOGY.md, IMPLEMENTATION.md, and PROCESS.md for the full rule set — an agent can act immediately from the block alone, but the references exist for complete context.
- **Language-agnostic.** Applies to any implementation language.
- **No implementation-specific guidance.** That belongs in the consuming project's own instructions.

### Test the change against the examples

After any change to rules, templates, or conventions, verify all three examples still conform. If they don't, update them as part of the same change — a rule change without corresponding example updates is incomplete.

## Developing examples

The `examples/` directory contains three reference specs that demonstrate the methodology. They are not illustrative sketches — they are complete, conformant specs that exercise every template and rule.

- **`link-checker`** — simple, at `1.0.0`: one root ability with sub-abilities and a Composition section, an HTTP transport decision, no state machine, an idempotency annotation, and scenarios covering every success criterion and root failure mode.
- **`content-moderation`** — complex, at `1.0.0`: state machine with orchestrator-managed state, four sub-abilities, a policy supplied as a root-level input with its own decision, multiple concept domains including enums, decisions for transport, policy source, scoring model, and reviewer channel, and scenarios covering every transition.
- **`notes-app`** — a draft below `1.0.0`: vision sections with criteria not yet covered, pending sections, a Composition section over a pending child, an open decision with options, a screen ability whose invariants pin UI behavior, a decision binding visual design, and a sub-ability delegated to `link-checker`.

Together they cover every construct the methodology defines. If a new construct is added to the methodology, at least one example must demonstrate it. Add a fourth example only if none of the existing three is a natural fit for the new construct.

### Example quality bar

- **Conformant.** Every example must pass the authoring rules in `METHODOLOGY.md` and use the templates in `CONVENTIONS.md` exactly. The two `1.0.0` examples must satisfy every structural readiness condition in `PROCESS.md`; `notes-app` must remain below `1.0.0` and is the only example permitted to contain `_Pending._` or `_Open._`. The examples are what users and agents look at to understand what a correct spec looks like.
- **Realistic.** The examples model believable problems. Do not simplify them to the point where they no longer exercise the constructs they demonstrate. Every value an invariant references — a threshold, a deadline, a limit — is an input, a concept type, or a decision, never an unstated assumption.
- **Cross-references resolve.** Every relative path link in ability Input/Output tables must point to a real `concept.md` file with a matching anchor. Verify all links after any structural change.
- **Internally consistent.** Types referenced in ability tables must exist in concept files. State names in scenarios must exist in the state machine. Ability names in success criteria and Composition sections must exist. Failure modes in ability tables must have conditions precise enough to test.
- **Canonical formatting.** Tables are padded, files end with one LF newline, and nothing else varies from what tooling renders.

## Versioning the methodology

AASDD uses integer versioning (`v1`, `v2`, …). A version bump means every spec written against the prior version may need to change. The bar for incrementing is high because downstream specs and tooling depend on stability.

**Breaking (requires version increment):**
- Changing required sections of a template
- Adding a new required construct
- Changing the meaning of an existing construct
- Removing a construct
- Changing naming conventions
- Modifying spec versioning rules
- Adding or changing a readiness condition

**Non-breaking (no version increment):**
- Adding an optional section to a template
- Adding guidance to `IMPLEMENTATION.md`
- Updating examples
- Expanding the wiki
- Fixing typos or clarifying wording without changing meaning
- Adding a new example

When in doubt, a change is breaking.

## Downstream dependencies

This repository has two categories of downstream consumers:

1. **Specs written by others.** Any project containing an AASDD spec depends on the rules, templates, and conventions defined here. Breaking changes force those specs to update.
2. **Tooling.** [aasdd-cli](https://github.com/smithyai/aasdd-cli) parses, validates, scaffolds, and transforms specs based on the templates and rules defined here. Template changes directly affect parser correctness, and readiness conditions 1 through 8 in `PROCESS.md` are its verification rules.

Be aware of both when making changes. A clarification that changes the meaning of a template section is not a typo fix — it is a breaking change that affects every spec and every tool.

## Boundaries

- **No code.** Implementation tooling lives in [aasdd-cli](https://github.com/smithyai/aasdd-cli). Do not add code, build scripts, or test suites to this repository.
- **No tutorials.** Practical guidance, walkthroughs, and tips belong in the [wiki](https://github.com/smithyai/aasdd/wiki). The core documents define rules; the wiki explains how to apply them. `PROCESS.md` defines the lifecycle as rules and conditions; the worked examples of applying it stay in the wiki.
- **No technology prescriptions.** The methodology is language-agnostic by design. Do not introduce language-specific advice, framework recommendations, or runtime assumptions into the core documents or examples.
- **No spec authoring here.** This repository defines how specs work — it does not contain a spec of its own (the examples are reference demonstrations, not operational specs).
