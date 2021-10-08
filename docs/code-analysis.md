# code analysis

[Analysis Tools Website](https://analysis-tools.dev/tools) lists a comprehensive set of analysis tools. You may find additional tools on the website [of shields](https://shields.io/) which provide the little badges for markdown files, which are often found on github.

- Code quality
  - Quality ands security [SonarQube](https://www.sonarqube.org/) or [SonarCloud](https://sonarcloud.io)
  - [codeclimate](https://docs.codeclimate.com/)
  - [semgrep](https://semgrep.dev/)
- Security
  - [lgtm](https://lgtm.com/) (is joining github)
  - Analyse Containers, Filesystem, Git Repos [Trivy](https://github.com/aquasecurity/trivy)
  - Security for dependecies, code, containers and infrastructure-code [Snyk](https://support.snyk.io/hc/en-us?utm_campaign=docs&utm_medium=github&utm_source=full_docs)
  - check git repos for secrets [gitguardian](https://www.gitguardian.com/)
  - security check for used libraries [dependabot](https://dependabot.com/)
- Test coverage (may need additional client library which produce the reports in ci)
  - [codecov](https://github.com/aquasecurity/trivy)
  - [coverall](https://coveralls.io/)
  - Tools may need existing reports which are build in ci pipepline
    -  java
       - jacoco
       - cobertura
- [License check: fossa](https://fossa.com/)
- Uptime check: [uptimerobot](https://uptimerobot.com/#features)
- python
  - security [bandit](https://github.com/PyCQA/bandit)
- bash
  - [ShellCheck](https://github.com/koalaman/shellcheck)
