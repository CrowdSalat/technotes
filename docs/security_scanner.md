# Security scanner

## SBOM

A software bill of materials (SBOM) is an industry standard mechanism of surfacing metadata about dependencies in images or applications. [Source](https://paketo.io/docs/concepts/sbom/). An SBOM can be created based on the source code, binaries (language-specific artifacts or container images) or dynamically at runtime.

### formats

- [spdx from linux foundation](https://spdx.dev/)
- [cyclone dx from owasp](https://cyclonedx.org/)
- [syft from/with grype](https://github.com/anchore/syft)

### SBOM tools

Software Composition Analysis (SCA)

- [cdxgen](https://github.com/AppThreat/cdxgen) - cli to create CycloneDX for different package managers (no build binaries needed)
- [CLI to generate spdx sbom for different package managers](https://github.com/opensbom-generator/spdx-sbom-generator)
- [cyclonedx cli](https://github.com/CycloneDX/cyclonedx-cli) from owasp currently supports BOM analysis, modification, diffing, merging, format conversion, signing and verification.
- [DependencyTrack](https://dependencytrack.org/) continuous SBOM analysis platform from owasp
- [cve-bin-tool from intel](https://github.com/intel/cve-bin-tool#scanning-an-sbom-file-for-known-vulnerabilities) allows to check binary or sbom for CVEs
- [spdx-to-osv cli](https://github.com/spdx/spdx-to-osv/) allows to find Open Source Vulnerabilities in SBOM

### vulnerability scanner

These scanners usually allow to scan binaries, but some also allow to start form a existing SBOM file.

- [DependencyCheck](https://jeremylong.github.io/DependencyCheck/dependency-check-cli/index.html) scanner from owasp (cli or build plugins for maven, gradle, etc.)
- [Trivy](https://github.com/aquasecurity/trivy) multi purpose scanner.
- [Grype](https://github.com/anchore/grype) a vulnerability scanner for container images and filesystems
- [Clair](https://github.com/quay/clair) a vulnerability scanner for container images from redhat

### examples

```shell
# generate spdx sbom from maven project
spdx-sbom-generator
## creates: bom-Java-Maven.spdx

# scan spdx or cyclonedx sbom for vulnerabilities
trivy sbom bom-Java-Maven.spdx
## or 
grype sbom:/bom-Java-Maven.spdx

## generate sbom from jar
syft --output cyclonedx-json name.jar > sbom-cyclonedx.json
syft --output spdx-json name.jar > sbom-spdx.json

## scan jar for vulnerabilities
grype name.jar
```

## vulnerability databases

- [Common Vulnerabilities and Exposures (CVE)](https://cve.mitre.org/)
- [Open Source Vulnerabilities Database (OSV)](https://osv.dev/)
- [National Vulnerability Database (NVD)](https://nvd.nist.gov/)
- [Sonatype oss index](https://ossindex.sonatype.org/)

## Common Platform Enumeration (CPE) 

Is a structured naming scheme for information technology systems, software, and packages.
