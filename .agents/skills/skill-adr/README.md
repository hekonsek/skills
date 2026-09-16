# skill-adr: Lightweight ADR documentation skill for AI agents

An Agent Skill for creating, updating, and reviewing lightweight Markdown-based Architecture Decision Record (ADR) for software projects.

## Why this skill?

This skill intentionally creates simplified version of ADR that we find most useful for the majority of the projects. Our ADR format contains only the following sections:

- Context
- Decision
- Consequences
- Alternatives cosidered

## Why not addyosmani/agent-skills?

There is an excellent [documentation skill by Addy Osmani](https://github.com/addyosmani/agent-skills/blob/main/skills/documentation-and-adrs/SKILL.md) which includes ADR support, however it also covers more aspects of documentation (like code comments or changelogs) which we would like to cover in dedicated skills. However ADR format created by this skill is quite in sync with Addy Osmani skill. 

## Supported agents

This skill is also intentionally tool-agnostic. It follows the portable [Agent Skills](https://skill.md/) format and does not require Codex, OpenAI or any ADR-specific generator.

## License

This project is licensed under the [MIT License](LICENSE).
