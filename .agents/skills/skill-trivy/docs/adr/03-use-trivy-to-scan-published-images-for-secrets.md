# Use Trivy to Scan Published Images for Secrets

## Context

Container builds can copy credentials or generated configuration into an image.
Scanning project files alone does not verify the artifact we publish. We need a
secret check on the built image before distributing it.

## Decision

We will use Trivy image secret scanning against every container image we intend
to publish. After building and before pushing or releasing the image, scan the
exact build intended for publication:

```sh
trivy image --scanners secret --exit-code 1 myapp:release
```

Replace `myapp:release` with the built image reference. Use a unique build tag,
image ID, or digest as appropriate so the scanned artifact is the one published.
`--scanners secret` selects secret scanning and `--exit-code 1` makes findings
fail the check. Resolve real findings, rebuild, and scan the replacement image
before publication. Document narrowly scoped false-positive exceptions.

This complements the project filesystem scan in
[ADR 02](02-use-trivy-to-scan-projects-for-secrets.md).

Command reference: [Trivy image CLI](https://trivy.dev/docs/latest/guide/references/configuration/cli/trivy_image/).

## Consequences

Positive consequences:

- We check the artifact being distributed for secrets introduced by the build.
- A failing scan provides a clear signal to stop publication and fix the image.

Negative consequences:

- Scanning adds build time and requires Trivy to access the built image.
- Detection has coverage limits and false positives; a clean scan does not
  guarantee that the image contains no secrets.

## Alternatives Considered

**Scan project files only.** This misses credentials introduced during image
assembly or copied from outside the scanned project.

**Scan only after publication.** This can detect problems, but recipients may
already have downloaded an image containing credentials.
