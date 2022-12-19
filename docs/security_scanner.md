# Security scanner

## SBOM

A software bill of materials (SBOM) is an industry standard mechanism of surfacing metadata about dependencies in images or applications. [Source](https://paketo.io/docs/concepts/sbom/)

Standards:

- [spdx from linux foundation](https://spdx.dev/)
  - [CLI to generate spdx sbom for different package managers](https://github.com/opensbom-generator/spdx-sbom-generator) 
- [cyclone dx from owasp](https://cyclonedx.org/)
- [syft from/with grype](https://github.com/anchore/syft)

## Scanner

- [Grype](https://github.com/anchore/grype) a vulnerability scanner for container images and filesystems
- [Trivy](https://github.com/aquasecurity/trivy) multi purpose scanner.
- [Clair](https://github.com/quay/clair) from redhat
- [Dependency track](https://dependencytrack.org/) - sbom scanner from owasp
