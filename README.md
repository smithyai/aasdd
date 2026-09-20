# Ability-Anchored Spec-Driven Development

A methodology for building software from language-agnostic specifications structured around **abilities** — discrete, composable units of behavior with defined contracts. The specification is the single source of truth. Implementation follows.

**Version:** v2

## Documents

| Document                               | Contents                                                                                                   |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [METHODOLOGY.md](METHODOLOGY.md)       | What AASDD is, how specs are structured, authoring rules, and versioning                                   |
| [CONVENTIONS.md](CONVENTIONS.md)       | Naming conventions, file naming, formatting rules, and canonical file templates                            |
| [PROCESS.md](PROCESS.md)               | The spec lifecycle: authoring one level at a time, the readiness gate, the implementation run, and closing |
| [IMPLEMENTATION.md](IMPLEMENTATION.md) | Translating specs to code: ordering, testing, development cycle, and agent instructions                    |

## Examples

| Example                                                      | Description                                                                                                                                                     |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [examples/link-checker/](examples/link-checker/)             | Spec at `1.0.0`: one root ability with a composition table, an HTTP transport decision, and scenarios covering every success criterion and root failure mode    |
| [examples/content-moderation/](examples/content-moderation/) | Spec at `1.0.0`: a state machine, a policy supplied as input, four decisions, and scenarios covering every transition                                           |
| [examples/notes-app/](examples/notes-app/)                   | Draft spec below `1.0.0`: pending sections, an open decision with options, a screen ability with UI invariants, and a sub-ability delegated to the link checker |

## Tooling

| Tool                                               | Description                                                                                                                                                                                                                                    |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [aasdd-cli](https://github.com/smithyai/aasdd-cli) | CLI for working with AASDD specs: verify spec directories against conventions, export specs to and import them from a JSON representation, scaffold empty or example spec directories, diff and graph specs, and list supported AASDD versions |

## Wiki

The [wiki](https://github.com/smithyai/aasdd/wiki) has practical guidance, tips, and best practices for using AASDD:

- [Why "Abilities"](https://github.com/smithyai/aasdd/wiki/Why-Abilities) — Why this term instead of component, service, or module
- [What AASDD Is Not](https://github.com/smithyai/aasdd/wiki/What-AASDD-Is-Not) — Common misconceptions and boundaries
- [Using Agents](https://github.com/smithyai/aasdd/wiki/Using-Agents) — Spec authoring vs. spec-driven implementation
- [Authoring Specs with Agents](https://github.com/smithyai/aasdd/wiki/Authoring-Specs-with-Agents) — Why one-pass generation fails and how to work iteratively
- [Iterative Decomposition](https://github.com/smithyai/aasdd/wiki/Iterative-Decomposition) — Worked example of decomposing abilities from rough idea to leaf specs
- [From Spec to Implementation](https://github.com/smithyai/aasdd/wiki/From-Spec-to-Implementation) — How a completed spec translates to working code
- [Recovering from Spec Drift](https://github.com/smithyai/aasdd/wiki/Recovering-from-Spec-Drift) — How to detect and fix divergence between spec and implementation
- [Spec Coverage](https://github.com/smithyai/aasdd/wiki/Spec-Coverage) — Drift, unimplemented constructs, and how to measure both
- [Custom Sections](https://github.com/smithyai/aasdd/wiki/Custom-Sections) — Author-defined sections for figures, notes, and references
- [Structured Representations](https://github.com/smithyai/aasdd/wiki/Structured-Representations) — JSON and YAML equivalents of a markdown spec

## What changed in v2

- `spec.md` carries the vision: Purpose, Non-Goals, and Success Criteria, with each criterion traced to the root abilities that serve it and the scenarios that exercise it.
- A spec below `1.0.0` may mark required sections `_Pending._` and decisions `_Open._`, so a spec stays conformant while it is decomposed one level at a time.
- Decisions cover any choice the contracts leave open, product or technical, and an open decision lists its options.
- Non-leaf abilities carry a Composition section stating what flows between their sub-abilities, which makes implementation order mechanical.
- Scenarios are the acceptance suite: at `1.0.0` they cover every success criterion, every root failure mode, and every state machine transition.
- A sub-ability may be delegated to another spec.
- `1.0.0` means the readiness gate in [PROCESS.md](PROCESS.md) has passed, and the implementation run that follows needs no human review.
