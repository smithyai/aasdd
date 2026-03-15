# Ability-Anchored Spec-Driven Development

A methodology for building software from language-agnostic specifications structured around **abilities** — discrete, composable units of behavior with defined contracts. The specification is the single source of truth. Implementation follows.

**Version:** v1

## Documents

| Document                               | Contents                                                                                |
| -------------------------------------- | --------------------------------------------------------------------------------------- |
| [METHODOLOGY.md](METHODOLOGY.md)       | What AASDD is, how specs are structured, authoring rules, and versioning                |
| [CONVENTIONS.md](CONVENTIONS.md)       | Naming conventions, file naming, formatting rules, and canonical file templates         |
| [IMPLEMENTATION.md](IMPLEMENTATION.md) | Translating specs to code: ordering, testing, development cycle, and agent instructions |

## Examples

| Example                                                      | Description                                                                            |
| ------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| [examples/link-checker/](examples/link-checker/)             | Simple spec: one root ability, HTTP transport decision, sub-ability decomposition      |
| [examples/content-moderation/](examples/content-moderation/) | Spec with a state machine, multiple abilities, and a gRPC interaction channel decision |

## Tooling

| Tool                                               | Description                                                                                                                                                                                               |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [aasdd-cli](https://github.com/smithyai/aasdd-cli) | CLI for working with AASDD specs: validate spec directories against conventions, parse specs to/from a JSON representation, scaffold empty or example spec directories, and list supported AASDD versions |

## Wiki

The [wiki](https://github.com/smithyai/aasdd/wiki) has practical guidance, tips, and best practices for using AASDD:

- [Why "Abilities"](https://github.com/smithyai/aasdd/wiki/Why-Abilities) — Why this term instead of component, service, or module
- [What AASDD Is Not](https://github.com/smithyai/aasdd/wiki/What-AASDD-Is-Not) — Common misconceptions and boundaries
- [Using Agents](https://github.com/smithyai/aasdd/wiki/Using-Agents) — Spec authoring vs. spec-driven implementation
- [Authoring Specs with Agents](https://github.com/smithyai/aasdd/wiki/Authoring-Specs-with-Agents) — Why one-pass generation fails and how to work iteratively
- [From Spec to Implementation](https://github.com/smithyai/aasdd/wiki/From-Spec-to-Implementation) — How a completed spec translates to working code
- [Spec Coverage](https://github.com/smithyai/aasdd/wiki/Spec-Coverage) — Drift, unimplemented constructs, and how to measure both
- [Custom Sections](https://github.com/smithyai/aasdd/wiki/Custom-Sections) — Author-defined sections for figures, notes, and references
- [Structured Representations](https://github.com/smithyai/aasdd/wiki/Structured-Representations) — JSON and YAML equivalents of a markdown spec
