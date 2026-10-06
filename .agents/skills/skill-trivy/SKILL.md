---
name: skill-trivy
description: Best practices for using Trivy to scan working project files and container images. Apply during development and before publishing built images.
---

# Trivy best practicies

## Local Ubuntu Installation

Read [ADR 01](docs/adr/01-prefer-the-trivy-apt-repository-on-local-ubuntu.md)
when installing Trivy on local Ubuntu. Prefer the official Trivy APT repository;
Homebrew (`brew install trivy`) is an acceptable alternative. Verify the
installation with `trivy --version` before scanning.

## Project Files

Read [ADR 02](docs/adr/02-use-trivy-to-scan-projects-for-secrets.md) when working
on a project. Scan from its root during development and before sharing changes:

```sh
trivy fs --scanners secret --exit-code 1 .
```

## Container Images

Read [ADR 03](docs/adr/03-use-trivy-to-scan-published-images-for-secrets.md) when
preparing an image for publication. Scan the exact built artifact before pushing
or releasing it, replacing the example reference:

```sh
trivy image --scanners secret --exit-code 1 myapp:release
```

## Handle Results

Review findings and report affected paths and rule IDs without reproducing secret
values. Remove real secrets; rebuild affected images and rerun the relevant scan.
If a credential was exposed, flag the need for revocation or rotation. Keep
false-positive exceptions narrow and justified.

Treat scanner execution errors as incomplete checks. If Trivy or the target is
unavailable, report the blocker; do not report a clean scan. A filesystem scan
does not cover Git history, and a clean project scan does not replace an image
scan.
