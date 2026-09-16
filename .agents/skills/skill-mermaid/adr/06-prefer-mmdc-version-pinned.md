# Prefer mmdc Version Pinned

## Context

Running `mmdc` executes Mermaid CLI and its dependency tree. Allowing a package manager to resolve an unspecified version or the latest release can introduce incompatible code from a new minor or major release. This creates avoidable exposure to unexpected package updates and makes rendering less predictable.

Patch releases normally provide compatible bug and security fixes. The project should receive those fixes without moving beyond the selected minor release line.

The project may occasionally need a version outside the preferred minor line because of compatibility requirements, regressions, security fixes, or features unavailable in the preferred version. Such exceptions should remain possible, but they must not happen silently.

## Decision

We will pin `mmdc` to the `11.17.x` release line and use the latest available patch version in that line whenever we install Mermaid CLI. Package specifications must constrain resolution to `11.17.x` rather than selecting an unspecified version, `latest`, or a different minor or major version.

Before installing or running `mmdc`, an agent must identify the version that will be used and the latest available `11.17.x` patch. If the proposed version is an older `11.17.x` patch, the agent should suggest updating to the latest patch before proceeding.

Before an agent installs or runs `mmdc` at a version outside `11.17.x`, including an already installed older or newer version, it must identify the proposed version and ask the user for explicit confirmation. When asking, the agent must explain that the project pins the minor version to reduce exposure to unexpected package updates. It must offer the user a choice between explicitly accepting the identified alternative version and installing or using the latest `11.17.x` patch. The agent may proceed with the alternative version only after the user accepts it.

When changing the preferred minor release line for the project rather than making a one-time exception, we will review the new minor version and update this decision.

## Consequences

Positive consequences:

- Rendering does not silently adopt newly published minor or major Mermaid CLI changes.
- The minor-version pin reduces exposure to unexpected incompatible or supply-chain changes outside the selected release line.
- The project receives compatible bug and security fixes published for `11.17.x`.
- Version exceptions remain possible and visible to the user.
- Users receive the version-policy rationale and a preferred-version option when deciding whether to allow an exception.

Negative consequences:

- A newly published `11.17.x` patch may be selected without project-specific review, so builds are less reproducible than with an exact version.
- Agents need access to package metadata to determine whether a newer `11.17.x` patch is available.
- Agents must pause and present two version choices when only a version outside `11.17.x` is installed or when compatibility requires an exception.
- A minor-version pin does not guarantee that the selected artifact or its transitive dependencies are trustworthy; package integrity and provenance still matter.
- Maintaining the preferred minor line requires periodic review as Mermaid CLI evolves.

## Alternatives Considered

**Pin exactly to `11.17.0`**. This maximizes reproducibility, but it does not automatically pick up later bug and security fixes in the `11.17` release line.

**Always use the latest `mmdc` version**. This provides new features and fixes quickly, but it allows newly published minor and major changes to execute without project-specific review and can introduce incompatibilities.

**Allow any locally installed `mmdc` version without confirmation**. This is convenient and works well across varied environments, but it silently weakens the version policy and can execute unexpected code.
