# Ability-Anchored Spec-Driven Development

A methodology for building software from language-agnostic specifications structured around **abilities** — discrete, composable units of behavior with defined contracts. The specification is the single source of truth. Implementation follows.

**Version:** v1

## Documents

| Document | Contents |
| --- | --- |
| [METHODOLOGY.md](METHODOLOGY.md) | What AASDD is, how specs are structured, authoring rules, and versioning |
| [CONVENTIONS.md](CONVENTIONS.md) | Naming conventions, file naming, formatting rules, and canonical file templates |
| [IMPLEMENTATION.md](IMPLEMENTATION.md) | Translating specs to code: ordering, testing, development cycle, and agent instructions |

## Examples

| Example | Description |
| --- | --- |
| [examples/link-checker/](examples/link-checker/) | Simple spec: one root ability, HTTP transport decision, sub-ability decomposition |
| [examples/content-moderation/](examples/content-moderation/) | Spec with a state machine, multiple abilities, and a gRPC interaction channel decision |
