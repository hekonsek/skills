# Prefer mmdc Version Pinned to 11.17.0

## Context

Running `mmdc` executes Mermaid CLI and its dependency tree. Allowing a package manager to resolve an unspecified version, a version range, or the latest release can introduce code that has not been reviewed or previously used by this project. This creates avoidable exposure to supply chain attacks and makes rendering less reproducible.

The project may occasionally need an older or newer version because of compatibility requirements, regressions, security fixes, or features unavailable in the preferred version. Such exceptions should remain possible, but they must not happen silently.

## Decision

We will prefer `mmdc` version `11.17.0` and specify that exact version whenever we install or invoke Mermaid CLI. We will not use an unpinned version or a version range by default.

Before an agent installs or runs `mmdc` at any version other than `11.17.0`, including an already installed older or newer version, it must identify the proposed version and ask the user for explicit confirmation. When asking, the agent must explain that the project pins `mmdc` to reduce exposure to supply chain attacks from unexpected package updates. It must offer the user a choice between explicitly accepting the identified alternative version and installing or using the pinned `11.17.0` version. The agent may proceed with the alternative version only after the user accepts it.

When changing the preferred version for the project rather than making a one-time exception, we will review the new version and update this decision.

## Consequences

Positive consequences:

- Rendering does not silently adopt newly published Mermaid CLI code or dependency changes.
- The fixed version reduces exposure to supply chain attacks delivered through an unexpected package update.
- Local and automated rendering is more reproducible because the selected Mermaid CLI version is explicit.
- Version exceptions remain possible and visible to the user.
- Users receive the security rationale and a safe pinned-version option when deciding whether to allow an exception.

Negative consequences:

- Agents must pause and present the two version choices when only another version is installed or when compatibility requires an exception.
- Pinning can delay adoption of security fixes, bug fixes, and Mermaid features until the preferred version is reviewed and updated.
- A version pin does not guarantee that the pinned artifact or its transitive dependencies are trustworthy; package integrity and provenance still matter.
- Maintaining the preferred version requires periodic review as Mermaid CLI evolves.

## Alternatives Considered

**Always use the latest `mmdc` version**. This provides new features and fixes quickly, but it allows newly published code and dependency changes to execute without project-specific review and makes rendering less reproducible.

**Allow any locally installed `mmdc` version without confirmation**. This is convenient and works well across varied environments, but it silently weakens the version policy and can execute unexpected code.

**Require version `11.17.0` without exceptions**. This provides a simpler and stricter rule, but it prevents necessary compatibility work, urgent security upgrades, and deliberate testing of other versions.
