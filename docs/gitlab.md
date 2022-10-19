# Gitlab Runner

- [Guide how to install a **gitlab-runner**](https://medium.com/devops-with-valentine/how-to-configure-your-own-gitlab-runner-with-a-docker-executor-on-aws-ec2-d76c7be0660d)
- A gitlab runner can use different [**executors**](https://docs.gitlab.com/runner/executors/) e.g. shell, docker or kubernetes 
- [pipeline templates used by auto devops](https://gitlab.com/gitlab-org/gitlab/-/tree/master/lib/gitlab/ci/templates)

## ansible galaxy auth

Ansible-Galaxy does not support (gitlab) oauth tokens in the url. [See Issue](https://github.com/ansible/ansible/issues/17194)

One solution is to redirect the ssh git link in the ansible galaxy requirements.yaml to https and addtionally add a config for a git credential manager which adds the token for the redirected https url

```shell
git config --global credential.helper "store --file https://example.com/ansible-roles
git config --global url."https://example.com/ansible-roles".insteadOf "git@example.com:ansible-roles"
```

Sources:

- https://forum.gitlab.com/t/gilab-ci-ansible-galaxy-requirements/51515/4
- https://gist.github.com/wtfred/abeee62d6a3e4371dbb4b309322dac30
