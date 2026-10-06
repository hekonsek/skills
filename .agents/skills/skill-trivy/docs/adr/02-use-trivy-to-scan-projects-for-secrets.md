# Use Trivy to Scan Projects for Secrets

## Context

Projects we work on can accidentally contain credentials in source files, local
configuration, or generated files. We need a repeatable way to detect exposed
secrets while developing, before sharing changes.

## Decision

We will use Trivy filesystem secret scanning on the projects we work on. Run the
following command from the project root during development and before sharing
changes, or replace `.` with the project path:

```sh
trivy fs --scanners secret --exit-code 1 .
```

`--scanners secret` explicitly selects secret scanning. `--exit-code 1` makes
findings produce a nonzero exit code so the same command can serve as a CI check.
Review findings, remove real secrets, and rerun the scan. Any false-positive
exception should be narrow and document its justification.

This scans the current filesystem; it does not establish that Git history is
free of secrets. Container images have a separate scan decision in
[ADR 03](03-use-trivy-to-scan-published-images-for-secrets.md).

Command reference: [Trivy filesystem CLI](https://trivy.dev/docs/latest/guide/references/configuration/cli/trivy_filesystem/).

## Consequences

Positive consequences:

- We detect accidental secret exposure earlier in development.
- Developers and CI can use the same explicit scan command.

Negative consequences:

- Trivy must be available, and scanning adds time to the development workflow.
- Pattern-based detection can miss secrets and produce false positives that
  require review; a clean scan is not proof that no secrets exist.

## Alternatives Considered

**Manual review only.** Review remains useful, but it is too easy to overlook
credentials across many files.

**Scan container images only.** Image scanning does not cover project files
that never enter the image, so it cannot replace filesystem scanning.
