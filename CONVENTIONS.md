Naming conventions, file naming rules, formatting rules, and canonical file templates for AASDD specs.

## Naming

| Thing                             | Convention                         | Example                                       |
| --------------------------------- | ---------------------------------- | --------------------------------------------- |
| Spec names                        | Title Case noun phrase             | `Link Checker`, `Content Moderation`          |
| Ability names                     | PascalCase verb phrase             | `ParsePrompt`, `SnapshotWorkspace`            |
| Ability folders                   | kebab-case of the ability name     | `parse-prompt/`, `snapshot-workspace/`        |
| Type names                        | PascalCase noun                    | `WorkspaceSnapshot`, `ChangeSet`              |
| Concept domain names              | Title Case noun                    | `Workspace`, `Planning`                       |
| Concept folders                   | kebab-case of the domain name      | `workspace/`, `planning/`                     |
| Scenario names                    | Title Case phrase                  | `Happy Path`, `Max Retries Exhausted`         |
| Scenario folders                  | kebab-case of the scenario name    | `happy-path/`, `max-retries-exhausted/`       |
| Decision names                    | PascalCase noun phrase             | `DocumentTransport`, `InteractionChannel`     |
| Decision folders                  | kebab-case of the decision name    | `document-transport/`, `interaction-channel/` |
| State names                       | PascalCase adjective or participle | `Idle`, `Analyzing`, `Failed`, `Complete`     |
| Failure names                     | PascalCase noun phrase             | `UnparsableDocument`, `NoReviewerAvailable`   |
| Input, output, and property names | snake_case                         | `submission_id`, `token_count`                |

Every folder name is derived from the heading of the artifact it holds: lowercase the words and join them with hyphens, splitting PascalCase at each capital (`SnapshotWorkspace` → `snapshot-workspace`, `Happy Path` → `happy-path`). A concept folder derives from the domain name alone, without the word "domain".

Type names are canonical across spec and implementation — adapt only the casing to the language convention (`WorkspaceSnapshot` → `workspace_snapshot` in snake_case).

## File Naming

| Location                    | Artifact           |
| --------------------------- | ------------------ |
| Specification overview      | `spec.md`          |
| Ability spec (at any depth) | `ability.md`       |
| Concept domain              | `concept.md`       |
| Scenario                    | `scenario.md`      |
| State machine               | `state-machine.md` |
| Decision                    | `decision.md`      |

Spec artifacts use a fixed `<type>.md` filename — the directory name carries identity, the filename carries the type. All spec artifact files start at H2.

## Formatting Rules

- Line endings are LF. Every file ends with exactly one newline character.
- One blank line between every block (heading, paragraph, table, list, blockquote, code block); no consecutive blank lines.
- Tables use a separator row of hyphens with no `:` alignment markers. Every cell is padded with spaces so that the pipes of a column align, and each separator cell spans the column width. This padded form is canonical: it is what tooling renders, and a spec in any other spacing is reformatted to it.
- Table cells never contain a `|` character.
- All identifiers in table cells (input, output, property, state, ability, failure, and scenario names) are wrapped in backticks. Type references are links or plain scalar names.
- `—` (em dash) marks an empty cell: a state with no mapped ability, a Composition step performed by the parent itself, a success criterion with no scenario yet.
- `_None._` as the entire content of a section states that the section is intentionally empty. `_Pending._` as the entire content of a required section states that it has not been written yet. `_Open._` as the entire content of a Decision section states that the choice has not been made.

## File Templates

### `spec.md`

````markdown
## {SpecName}

**AASDD:** v2
**Version:** {X}.{Y}.{Z}

{One sentence describing what this spec covers.}

### Purpose

{Who the system is for and what problem it solves. One short paragraph.}

### Non-Goals

- {Something the system deliberately does not do}

### Success Criteria

| Criterion                                       | Abilities       | Scenarios           |
| ----------------------------------------------- | --------------- | ------------------- |
| {An outcome observable from outside the system} | `{RootAbility}` | `{scenario-folder}` |

### Invariants

- {Cross-cutting invariant that applies to the spec as a whole}
````

Purpose, Non-Goals, Success Criteria, and Invariants are required. Non-Goals may be `_None._`. The Abilities column lists root ability names; the Scenarios column lists scenario folder names, or `—` while the spec is below `1.0.0` and no scenario exists yet.

Optional sections (in order when present):

````markdown
### Failure Modes

| Failure         | Condition                    | Effect         |
| --------------- | ---------------------------- | -------------- |
| `{FailureName}` | {When this condition occurs} | {What happens} |
````

### `ability.md`

Required sections in this order. Optional sections omitted entirely when not applicable.

````markdown
## {AbilityName}

{One sentence stating what this ability does.}

### Inputs

| Name           | Type                                                 | Description   |
| -------------- | ---------------------------------------------------- | ------------- |
| `{input_name}` | [{TypeName}]({relative/path/to/concept.md#typename}) | {Description} |

### Outputs

| Name            | Type                                                 | Description   |
| --------------- | ---------------------------------------------------- | ------------- |
| `{output_name}` | [{TypeName}]({relative/path/to/concept.md#typename}) | {Description} |

### Invariants

- {Invariant statement}

### Failure Modes

| Failure         | Condition                    | Effect         |
| --------------- | ---------------------------- | -------------- |
| `{FailureName}` | {When this condition occurs} | {What happens} |
````

Inputs may be `_None._` when the ability takes no caller-supplied parameters; Failure Modes may be `_None._` when nothing can prevent the ability from producing its output. Any required section may be `_Pending._` while the spec is below `1.0.0`.

Optional sections (in order when present):

````markdown
### Idempotency

{The condition under which repeated invocation yields identical outputs.}

### Composition

| Step | Ability        | Consumes               | Produces          |
| ---- | -------------- | ---------------------- | ----------------- |
| 1    | `{SubAbility}` | `{input}` from parent  | `{output}`        |
| 2    | `{SubAbility}` | `{input}` from step 1  | `{output}`        |
| 3    | —              | `{output}` from step 2 | `{parent_output}` |
````

Composition is required for every non-leaf ability at `1.0.0` and above, except the root ability of a spec with a state machine. Each sub-ability appears in exactly one row. The Consumes cell names each input and its source — `from parent` or `from step N` where `N` is an earlier step — and may express iteration as `each {item} in {list} from step N`. A row whose Ability is `—` is work the parent performs itself. The final rows produce the parent's outputs.

After all required and recognized optional sections, any number of custom `###` sections may follow. Custom sections have no fixed template — their names and content are author-defined. This applies to all spec file types.

#### Delegated abilities

A sub-ability defined by another spec contains only the following, and no other sections:

````markdown
## {AbilityName}

{One sentence stating what this ability does.}

**Spec:** {relative path or URL of the delegated spec directory}
**Version:** {X}.{Y}.{Z}
````

The version is the exact version of the delegated spec the parent was authored against. The delegated spec must have exactly one root ability, whose contract is this ability's contract.

#### Type references

- **Scalar types** (`text`, `number`, `boolean`, `timestamp`) — plain text, not linked
- **Same-spec types** — `[TypeName](relative/path/to/concept.md#typename)`
- **Structural modifiers** — prefix: `list of TypeName`, `optional TypeName`, `map of KeyType to ValueType`; apply to any type including cross-spec types
- **Types from another spec** — plain text with a description noting external definition; no linking

### `concept.md`

````markdown
## {DomainName} domain

{One-sentence description of what types this domain contains.}

### {TypeName}

{One-sentence or short-paragraph description of what this type represents.}

#### Properties

| Name              | Type                      | Description   |
| ----------------- | ------------------------- | ------------- |
| `{property_name}` | [{TypeName}](#{typename}) | {Description} |
| `{property_name}` | text                      | {Description} |
````

Enum types (see [METHODOLOGY.md](METHODOLOGY.md#concepts)) omit `#### Properties` and use a Value/Meaning table instead:

````markdown
### {EnumTypeName}

{One-sentence description.}

| Value         | Meaning                 |
| ------------- | ----------------------- |
| `{ValueName}` | {What this value means} |
````

Type column conventions: scalar types are plain text, same-file references use `[TypeName](#typename)`, same-spec references use `[TypeName](../domain/concept.md#typename)`.

### `scenario.md`

````markdown
## {ScenarioName}

{One sentence describing what makes this scenario distinct.}

> `{Node1}` → `{Node2}` → `{Node3}`

- {Notable behavior or outcome}
````

Trace nodes are state names when the spec defines a state machine, and ability names otherwise. An ability trace begins with the root ability the caller invokes, followed by the sub-abilities invoked, in order; a sub-ability invoked once per item appears once.

Divergences use `—` (em dash). The condition is a transition trigger or a failure mode name. In an ability trace, the node after the condition is the ability that handles the failure, or the root ability when the failure propagates to the caller:

````markdown
> `{Node1}` → `{Node2}` — {ConditionName} → `{Node3}`
````

Optional `### Example` section (in order when present):

````markdown
### Example

{Concrete real-world instance of this scenario, written as prose.}
````

### `state-machine.md`

A `state-machine.md` appears at the spec root, alongside `spec.md`, coordinating the top-level lifecycle. A spec may define at most one state machine.

Sections appear in this exact order. Optional sections omitted entirely when not applicable.

````markdown
## State Machine

{One sentence describing what this machine orchestrates.}

```mermaid
stateDiagram-v2
    [*] --> {State1}
    {State1} --> {State2}: {trigger}
    {State2} --> [*]
```

### Orchestrator

{One-paragraph description of who owns transition logic and what the orchestrator does.}

#### Orchestrator-Managed State

| Name              | Type                                                   | Description   |
| ----------------- | ------------------------------------------------------ | ------------- |
| `{variable_name}` | [{TypeName}]({relative/path/to/concept.md#{typename}}) | {Description} |

### States

| State         | Ability         | Description                         |
| ------------- | --------------- | ----------------------------------- |
| `{StateName}` | `{AbilityName}` | {Description}                       |
| `{StateName}` | —               | {Description of control flow state} |

### Transitions

| From          | To          | Trigger                               | Data Passed Forward |
| ------------- | ----------- | ------------------------------------- | ------------------- |
| `{FromState}` | `{ToState}` | `{TriggerName}` — {trigger condition} | {Data description}  |

### Transition Rules

- {Cross-cutting constraint}

### Exceptional Flows

#### {FlowName}

{Prose describing recovery behavior or non-standard lifecycle events.}
````

The diagram is required. Use any diagram syntax that renders in your environment; the template uses Mermaid as a common default. Each Transitions row has exactly one named trigger; two transitions with the same From and To states are two rows. `#### Orchestrator-Managed State` is present only when the orchestrator maintains state across the lifecycle. `### Exceptional Flows` is optional.

### `decisions/{decision}/decision.md`

A decision for one choice the contracts leave open: the transport for an input that crosses a system boundary, the storage mechanism for persistent state, the model or service powering a capability, or a question of behavior the implementation would otherwise settle on its own. Each decision file covers exactly one choice. **Context** (which abilities the decision constrains and why a choice is needed), **Requirement** (what the implementation must provide), and **Decision** (the chosen approach) are required. **Options** is required while the decision is open and optional once it is closed.

````markdown
## {DecisionName}

### Context

{Which abilities this decision constrains, named in backticks, and why a choice is needed.}

### Requirement

{What capability the implementation must provide to satisfy the spec.}

### Options

- {A candidate approach and its trade-off}

### Decision

{The chosen approach and brief reasoning.}
````

An open decision has `_Open._` as the entire content of its Decision section, and is permitted only while the spec is below `1.0.0`.
