# Docker build

## multi-platform on macos m1 with colima

```shell
brew install docker-buildx
# Follow the caveats mentioned in the install instructions:
mkdir -p ~/.docker/cli-plugins
ln -sfn $(which docker-buildx) ~/.docker/cli-plugins/docker-buildx

# set build env
docker buildx create --use colima

# test
docker buildx install
```

## mutli arch local on m2

```shell
docker login
docker-buildx build --platform linux/amd64,linux/arm64 --push -t crowdsalat/sample:0.0.1 -t crowdsalat/sample:latest .
```
