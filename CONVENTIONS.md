Naming conventions, file naming rules, and canonical file templates for AASDD specs.

## Naming

| Thing            | Convention                         | Example                                       |
| ---------------- | ---------------------------------- | --------------------------------------------- |
| Ability names    | PascalCase verb phrase             | `ParsePrompt`, `SnapshotWorkspace`            |
| Ability folders  | kebab-case                         | `parse-prompt/`, `snapshot-workspace/`        |
| Type names       | PascalCase noun                    | `WorkspaceSnapshot`, `ChangeSet`              |
| Concept folders  | kebab-case domain                  | `workspace/`, `planning/`                     |
| Scenario folders | kebab-case description             | `happy-path/`, `max-retries-exhausted/`       |
| Decision names   | PascalCase noun phrase             | `DocumentTransport`, `InteractionChannel`     |
| Decision folders | kebab-case description             | `document-transport/`, `interaction-channel/` |
| State names      | PascalCase adjective or participle | `Idle`, `Analyzing`, `Failed`, `Complete`     |

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

- One blank line between every section (heading, paragraph, table, list, blockquote, code block)
- No trailing blank lines at end of file
- No consecutive blank lines
- Tables use `| --- |` separator rows (no `:` alignment markers)
- All identifiers in table cells (input names, output names, property names, state names, failure names) are wrapped in backticks
- The Ability column in state tables uses backticks for ability names; `—` (em dash) marks states with no mapped ability

## File Templates

### `spec.md`

````markdown
## {SpecName}

**AASDD:** v{N}
**Version:** {X}.{Y}.{Z}
**Summary:** {One sentence describing what this spec covers.}

### Invariants

- {Cross-cutting invariant that applies to the spec as a whole}
````

Optional sections (in order when present):

````markdown
### Failure Modes

| Failure         | Condition                    | Effect         |
| --------------- | ---------------------------- | -------------- |
| `{FailureName}` | {When this condition occurs} | {What happens} |
````

````markdown
### Visualization

{A diagram or other visual representation of the spec's structure.}
````

### `ability.md`

Required sections in this order. Optional sections omitted entirely when not applicable.

````markdown
## {AbilityName}

**Purpose:** {One sentence stating what this ability does.}

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

Optional sections (in order when present):

````markdown
### Idempotency

{Brief statement of idempotency behavior.}

### Visualization

{A diagram or other visual representation of the ability's sub-ability structure.}
````

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

Enum types omit `#### Properties` and use a Value/Meaning table instead:

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

**Description:** {One sentence describing what makes this scenario distinct.}

> `{State1}` → `{State2}` → `{State3}`

- {Notable behavior or outcome}
````

Divergences use `—` (em dash):

````markdown
> `{State1}` → `{State2}` — {ConditionName} → `{State3}`
````

Trace nodes are state names when the spec defines a state machine; use ability names when it does not. Every state name used in a trace must exist in the relevant `state-machine.md`.

Optional `### Example` section (in order when present):

````markdown
### Example

{Concrete real-world instance of this scenario, written as prose.}
````

### `state-machine.md`

A `state-machine.md` may appear at the spec root (coordinating the top-level lifecycle) or inside any ability directory (scoped to that ability's internal orchestration). Both follow the same template. A sub-ability's `state-machine.md` is invisible to anything outside that ability.

Sections appear in this exact order. Optional sections omitted entirely when not applicable.

````markdown
## State Machine

**Summary:** {One sentence describing what this machine orchestrates.}

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

| From          | To          | Trigger             | Data Passed Forward |
| ------------- | ----------- | ------------------- | ------------------- |
| `{FromState}` | `{ToState}` | {Trigger condition} | {Data description}  |

### Transition Rules

- {Cross-cutting constraint}

### Exceptional Flows

#### {FlowName}

{Prose describing recovery behavior or non-standard lifecycle events.}
````

The diagram is required. Use any diagram syntax that renders in your environment; the template uses Mermaid as a common default. `#### Orchestrator-Managed State` is present only when the orchestrator maintains state across the lifecycle. `### Exceptional Flows` is optional.

### `decisions/{decision}/decision.md`

A technology decision for one open choice the spec leaves to the implementer. Each decision file covers exactly one choice: the transport for an input that crosses a system boundary, the storage mechanism for persistent state, the model or service powering a capability. The three sections — **Context** (why a decision is needed), **Requirement** (what the implementation must provide), and **Decision** (the chosen approach) — are all required.

````markdown
## {DecisionName}

### Context

{What spec concept this relates to and why a technology decision is needed.}

### Requirement

{What capability the implementation must provide to satisfy the spec.}

### Decision

{The chosen approach and brief reasoning.}
````
