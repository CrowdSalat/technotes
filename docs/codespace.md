# codespaces / devcontainers

[Official documentation](https://docs.github.com/en/codespaces/overview)

## configure new one with wizard

In vscode prompt: `> add dev container configuration files`

[See](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/introduction-to-dev-containers#using-a-predefined-dev-container-configuration)

## use own Dockerfile

If you use a custom dockerifle add prebuilts to keep the startup snappy. Otherwise codespaces will build it all over again each time you open a codespace.
