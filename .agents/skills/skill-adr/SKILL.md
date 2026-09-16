---
name: skill-adr
description: Creates, updates, and reviews Markdown-based ADR (Architecture Decision Record) documentation projects. Use when working with DRs or architecture documentation. Or when you need to record an architectural decision context for future engineers or AI agents working on the project. 
---

# ADR Documentation for software projects

## When to use it 

Use this skill to create or maintain Architecture Decision Record (ADR) collections that are plain Markdown repositories or Markdown documentation folders inside an existing repository.

Create ADR when:
- choosing an architectural decision that needs to be documented
- choosing framework or library
- selecting authentication strategy
- making any decision that will be difficult or costly to change later or reverse

### When not to use it

Don't document code that is self-explanatory or trivial.

### Example of prompts triggering this skill

```
Create ADR for <DECISION>.
```

```
Propose what ADRs could be extracted from this project.
```

```
Improve formatting of existing ADRs.
```

## Handling existing conventions

Always try to follow the conventions of the existing ADR collection. If no conventions exist, use the conventions described in this document. Suggest user that you can convert existing ADRs to the conventions described here if they are not already following them, but always ask for confirmation before performing any significant refactoring or renaming of existing ADRs.

## Workflow

1. Identify the ADR collection domain and scope.
2. Create or update the project layout.
3. Add ADR Markdown files under the project's ADR directory.
4. Name each ADR file from its sequence number, optional category, and slugified title.
5. Write each ADR with the required Markdown sections.
6. Validate the Markdown structure and naming conventions.
7. Suggest ADR file refactoring or renaming if existing ADRs do not follow the conventions, but only after user confirmation.

## Project Layout

Use this repository structure:

```
.
|-- docs/adr/
|   |-- 01-use-typescript-instead-of-javascript.md
|   |-- 02-use-options-object-for-optional-parameters.md
|   `-- security/
|       `-- security_01-use-execfile-instead-of-exec.md
```

Required:

- `docs/adr/`: stores ADR Markdown files.

Optional:

- `docs/adr/<category>/`: groups ADRs by category, such as `security`. Categories can be nested, such as `docs/adr/security/authentication/`.

### ADR Location

For a new ADR-only repository, place ADR files under `docs/adr/`.

For an existing repository, first look for an established ADR location and use
it if present. Common alternatives include `adr/` in root of the project, `docs/architecture/adr/`,
and `architecture/decisions/`. Do not move existing ADRs unless the user asks for
a layout migration.

General ADRs are direct children:

```
docs/adr/01-use-typescript-instead-of-javascript.md
docs/adr/02-use-options-object-for-optional-parameters.md
```

Category-specific ADRs live in subdirectories:

```
docs/adr/security/security_01-use-execfile-instead-of-exec.md
```

### ADR File Names

General ADRs use:

```text
NN-slugified-markdown-heading.md
```

Rules:

- `NN` is a zero-padded sequence number.
- Use the least sequence width >= 2 that still sorts alphanumerically within the
  directory, such as `01` through `99` or `001` through `999`.
- Sequence numbers are monotonic within the directory.
- Derive the slug from the ADR `#` heading.
- Use lowercase kebab case for the slug.
- Use the `.md` extension.

The filename slug must match the heading:

```text
01-use-typescript-instead-of-javascript.md
```

```md
# Use TypeScript Instead of JavaScript
```

## ADR Document Format

Each ADR is one Markdown document with exactly one top-level heading and these
sections:

```md
# Use TypeScript Instead of JavaScript

## Context

Describe the situation, constraints, forces, and problem being solved.

## Decision

State the selected decision clearly.

## Consequences

Positive consequences:

- Describe a benefit of the decision.

Negative consequences:

- Describe a trade-off, cost, risk, operational burden, migration impact, or limitation of the decision.

## Alternatives Considered

**Alternative 1**. Describe the relevant alternative and why it was not selected.

**Alternative 2**. Describe the relevant alternative and why it was not selected.
```

### Writing Style

Write ADRs in a concise, direct, practical style.

- Explain project context before stating the decision.
- Use first person plural (`We will ...`) for accepted decisions.
- Document consequences as two separate lists: positive consequences and
  negative consequences.
- Actively look for negative consequences even when the decision is clearly
  beneficial. Do not leave the negative list empty or document only positive
  outcomes.
- Prefer concrete examples when the decision affects source layout, command
  usage, dependencies, or security posture.
- Keep each ADR focused on one decision (if an ADR is too broad, ask the user if they want to split it into multiple ADRs).
- Avoid implementation details that do not affect the decision.

## Validation

Before finishing ADR work, validate the files directly:

- Check that each ADR file is under the selected ADR directory.
- Check that filenames sort correctly and match their `#` heading slug.
- Check that every ADR has exactly one top-level `#` heading.
- Check that every ADR includes `Context`, `Decision`, `Consequences`, and
  `Alternatives Considered` sections.
- Check that every `Consequences` section contains separate positive and
  negative consequence lists.
- Check that the negative consequences list documents at least one meaningful
  trade-off, cost, risk, operational burden, migration impact, or limitation.
- Run existing Markdown lint, link-check, formatting, or documentation CI tools
  when the repository already provides them.
