# Gitpod

- [Dashboard](https://gitpod.io/workspaces/)
- [Gitpodifying — The Ultimate Guide](https://www.gitpod.io/blog/gitpodify/)

## start a workspace

To start workspace in gitpod:

  1. use gitpod firefox or chrome extension
  2. prefix git repository url with: `gitpod.io/#`

## configure workspace (gitpodify your project)

1. .gitpod.yml in the opened repository (run `gp init` to create one)
2. .gitpod.yaml in the central [definitely-gp](https://github.com/gitpod-io/definitely-gp) repository
3. automagically inferred gitpod config

To scaffold a .gitpod.yml use [gitpod cli](https://www.gitpod.io/docs/command-line-interface/) in a started gitpod workspace: `gp init`

### configure used container image

- configure custom image in .gitpod.Dockerfile and reference it in .gitpod.yml:
- configure image from Dockerhub: `image: image:tag`
- if non image is set the default [gitpod workspace image](https://github.com/gitpod-io/workspace-images/blob/master/full/Dockerfile) is used 

```yaml
# your own image
image:
  file: .gitpod.Dockerfile

# dockerhub image
image: node:alpine
```

*For compatibility reasons you may want to use a gitpod workspace image as base image*

### configure prebuild and start command

[Start tasks](https://www.gitpod.io/docs/config-start-tasks) contains different kinds of commands (before (command), init (command), (plain) command).

are executed in a defined order and only in defined start modes.

- Init command is run when creating a workspace (container) not when restarting or creating a snapshot
- (Plain) commands are run when starting a workspace (container)
- You can configure that the init command is run every commit. This is called [prebuild](https://www.gitpod.io/docs/prebuilds/) and can be configured with git webhooks or vscode extensions. 

```yaml
tasks:
  - init: | 
      yarn
      yarn build
    command: yarn dev --host 0.0.0.0
    # to execute task in parallel just add an additional entry to the tasks list (blocking is currently not natively supported)
  - command: echo "hello concurrency, i do not need yarn build so i can be run concurrently"

```

## configure used extension 

[Official documentation](https://www.gitpod.io/docs/vscode-extensions/)


To add a extension just go to the extension tab, click on the extension you want to add and click 'Add to .gitpod.yml' from the menu symbol. 


To view build in extension type @builtin in the extension tab search bar.