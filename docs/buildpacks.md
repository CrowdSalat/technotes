# Buildpacks

## tools

- [pack cli](https://github.com/buildpacks/pack) 
- Paketo Buildpacks implement the Buildpack API described in the [Cloud Native Buildpacks Specification](https://github.com/buildpacks/spec).

## terms

- A **builder (image)** contains a set of buildpacks and a stack
- a **buildpack** examine your app source code, identify and gather dependencies, and output OCI compliant app and dependency layers. It operates in two phases: detect and build.
- a **stack (image)** consist of build image and a run image
