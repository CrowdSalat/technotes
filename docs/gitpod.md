# Gitpod


Is a online ide which utilizes theano. It allows to define ephemeral development workspaces based on a git repository and and a container image

## features

- every workspace runs in a container. So you can start developing with a working and prebuild workspace. 
- you can share your workspace with others online 
- adapt the workspace for you needs
    - general configuration is in [.gitpod](https://www.gitpod.io/docs/config-gitpod-file/) file
    - you can also create your [own container image](https://www.gitpod.io/docs/config-docker/) for the workspace
    - you can test your changes of the theano config with the 'Test drive configuration' option in the 'Project Setup' view

## create a new workspace 

### wihout config

1. To start a **new theano workspace** open `https://www.gitpod.io/#<URL_TO_REPO>` in your browser e.g. `https://www.gitpod.io/#https://github.com/CrowdSalat/technotes`. You can also [address a specific branch or commit](https://www.gitpod.io/docs/context-urls/) 
2. You may need to request additional rights from your git provider to read and later write to the repository. You can grant rights to gitpod [https://gitpod.io/access-control/](https://gitpod.io/access-control/). 
3. Start developing as you are used to

### with config

1. To start a **new theano workspace** open: `https://www.gitpod.io/#<URL_TO_REPO>` in your browser e.g. `https://www.gitpod.io/#https://github.com/CrowdSalat/technotes`. You can also [address a specific branch or commit](https://www.gitpod.io/docs/context-urls/)
2. You may need to request additional rights from your git provider to read and later write to the repository. You can grant rights to gitpod [https://gitpod.io/access-control/](https://gitpod.io/access-control/). 
3. Use theano 'Project Setup' (symbol on the right side) to configure the workspace:
    - .gitpod.yml defines which commands should be initially run in the workspace
    - .gitpod.Dockerfile is used to **define the container image** in which the workspace runs. Theres is a list of usable base images from gitpod which it recommend you.
    - **Use 'Test drive configuration' to test you setup**. It will create a branch in you repo and create a new theano workspace from it. If the workspace builds from this without errors this step can be seen as succesful.
    - if you are happy with you theano config changes **create a pull request** to remote master and merge it to master 
4. Start developing as you are used to