---
name: skill-mermaid
description: Create and edit Mermaid architecture diagrams.
---

# Mermaid architecture diagrams

Produce approachable Mermaid diagrams.

## References

- Use [api.mmd](references/examples/api.mmd) as a reference for modeling an API architecture. For example for user consuming an API that interacts with a database or other systems.
- Use [events.mmd](references/examples/events.mmd) as a reference for modeling event-driven architectures. For example for system consisting of producers, message queues, and consumers.

## ADRs

- Use [this ADR](adr/01-prefer-hand-drawn-style-for-mermaid-diagrams.md) when deciding on the visual style for Mermaid diagrams.
- Use [this ADR](adr/02-prioritize-mmdc-rendering-for-mermaid-diagrams.md) when deciding on the preferred rendering tool for Mermaid diagrams.
- Use [this ADR](adr/03-place-mermaid-comments-on-separate-lines.md) when deciding on the comment syntax for Mermaid diagrams.
- Use [this ADR](adr/04-prefer-svg-over-png-for-rendered-mermaid-diagrams.md) when deciding on the default output format for Mermaid diagrams.
- Use [this ADR](adr/05-use-elk-layout-for-complex-flowcharts.md) when choosing a layout for complex flowchart diagrams.
- Use [this ADR](adr/06-prefer-mmdc-version-pinned.md) when installing or invoking `mmdc`. Stay on the `11.17.x` release line and prefer its latest available patch. If an older `11.17.x` patch is installed, identify both versions and suggest updating to the latest patch. Before using a version outside `11.17.x`, identify it and explain that the minor-version pin reduces exposure to unexpected package updates. Ask the user to choose between accepting that version and using the latest `11.17.x` patch; use the alternative version only after explicit acceptance.
