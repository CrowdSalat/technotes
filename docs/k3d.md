# k3d

## use local image

k3d cli allows to import local images to k3d. You need to set **imagePullPolicy: IfNotPresent** in the pod definition. Otherwise it tries to pull it from Dockerhub.

```shell
# k3d cluster create local
k3d image import IMAGENAME --cluster local
```
