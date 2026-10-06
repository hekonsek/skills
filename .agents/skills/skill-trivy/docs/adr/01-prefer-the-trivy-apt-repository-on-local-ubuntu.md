# Prefer the Trivy APT Repository on Local Ubuntu

## Context

We need Trivy available on local Ubuntu development machines to scan project
files and container images. A package manager provides a repeatable installation
and upgrade path without manually replacing downloaded binaries.

## Decision

We will prefer installing Trivy from its official APT repository on local
Ubuntu machines. Configure the repository and its signing key using the
[official Debian/Ubuntu installation instructions](https://trivy.dev/docs/latest/getting-started/installation/#debianubuntu-official),
then install the package:

```sh
sudo apt-get update
sudo apt-get install trivy
```

Alternatively, we will use Homebrew when it better fits the local development
environment, particularly when Homebrew is already in use:

```sh
brew install trivy
```

Homebrew is also an [officially documented installation option for Linux](https://trivy.dev/docs/latest/getting-started/installation/#homebrew-official).
Use one package manager to maintain the local installation and verify that
`trivy --version` resolves to the expected executable before scanning.

This preference applies to local Ubuntu development machines. The scanning
workflows are documented in [ADR 02](02-use-trivy-to-scan-projects-for-secrets.md)
and [ADR 03](03-use-trivy-to-scan-published-images-for-secrets.md).

## Consequences

Positive consequences:

- Trivy installation and upgrades fit Ubuntu's existing package management.
- Homebrew offers an alternative for developers who already use it.

Negative consequences:

- Configuring the APT repository and installing packages requires administrator
  privileges and access to the repository.
- Supporting two installation options means package versions and executable
  paths can differ between development machines.

## Alternatives Considered

**Homebrew as the default.** It remains an acceptable alternative, but APT fits
local Ubuntu machines without introducing another package manager.

**Manually downloaded binaries or DEB packages.** These require a separate
manual upgrade process, so we prefer a maintained package-manager installation.
