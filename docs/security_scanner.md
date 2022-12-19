# Security scanner

## SBOM

A software bill of materials (SBOM) is an industry standard mechanism of surfacing metadata about dependencies in images or applications. [Source](https://paketo.io/docs/concepts/sbom/).

Standards:

- [spdx from linux foundation](https://spdx.dev/)
- [cyclone dx from owasp](https://cyclonedx.org/)
- [syft from/with grype](https://github.com/anchore/syft)

Tools:

- [CLI to generate spdx sbom for different package managers](https://github.com/opensbom-generator/spdx-sbom-generator)
- [cyclonedx cli](https://github.com/CycloneDX/cyclonedx-cli) from owasp currently supports BOM analysis, modification, diffing, merging, format conversion, signing and verification.

## Scanner

These scanners usually allow to scan binaries, but some also allow to start form a existing SBOM file.

- [Grype](https://github.com/anchore/grype) a vulnerability scanner for container images and filesystems
- [Trivy](https://github.com/aquasecurity/trivy) multi purpose scanner.
- [Clair](https://github.com/quay/clair) container image scannerfrom redhat
- [Dependency track](https://jeremylong.github.io/DependencyCheck/dependency-check-cli/index.html) scanner from owasp (cli or build plugins for maven, gradle, etc.)


## Databases

- [Common Vulnerabilities and Exposures (CVE)](https://cve.mitre.org/)
- [Open Source Vulnerabilities Database (OSV)](https://osv.dev/)
- [National Vulnerability Database (NVD)](https://nvd.nist.gov/)
- [Sonatype oss index](https://ossindex.sonatype.org/)

## Common Platform Enumeration (CPE) 

Is a structured naming scheme for information technology systems, software, and packages.
