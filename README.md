# debian-docker
Dockerfile for Debian with basic tools

# Dockerfile source

https://github.com/nilcons/debian-docker/blob/master/Dockerfile

# Image registries we publish in

https://github.com/nilcons/debian-docker/pkgs/container/debian (`docker pull ghcr.io/nilcons/debian:latest`)

https://hub.docker.com/r/nilcons/debian  (`docker pull nilcons/debian:latest`)

# Automated build

See `.github/workflows/docker-build-push.yaml`.

We build for: amd64, armv7, arm64.

Built packages are tagged with `latest` (Docker Hub and GHCR) and with `sha-<githash>-<date>` (GHCR only).

Rebuilds happen on the fourth of every month even if no changes in the `Dockerfile`.

# Testing the ARM and ARM64 builds on your AMD64 workstation

Idea stolen from: https://www.stereolabs.com/docs/docker/building-arm-container-on-x86/

```
# Install QEMU
sudo apt-get install qemu-user binfmt-support qemu-user-static
# This step will execute the registering scripts
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
# Run the tests
DOCKER_DEFAULT_PLATFORM=linux/amd64  docker run --pull=always --rm -it nilcons/debian uname -m
DOCKER_DEFAULT_PLATFORM=linux/arm/v7 docker run --pull=always --rm -it nilcons/debian uname -m
DOCKER_DEFAULT_PLATFORM=linux/arm64  docker run --pull=always --rm -it nilcons/debian uname -m
```

# Hacking info

## Get current tags

```
docker run -it --rm ghcr.io/regclient/regctl tag ls ghcr.io/nilcons/debian
docker run -it --rm ghcr.io/regclient/regctl tag ls docker.io/nilcons/debian
```

## Reading multi-arch index

```
docker run -it --rm ghcr.io/regclient/regctl image manifest ghcr.io/nilcons/debian
docker run -it --rm ghcr.io/regclient/regctl image manifest docker.io/nilcons/debian
```

## Reading manifest for a concrete architecture

```
docker run -it --rm ghcr.io/regclient/regctl image manifest -p linux/arm/v7 ghcr.io/nilcons/debian
docker run -it --rm ghcr.io/regclient/regctl image manifest -p linux/arm/v7 docker.io/nilcons/debian
```

## Get inspect output for an image (downloads blobs and looks inside)

```
docker run -it --rm ghcr.io/regclient/regctl image inspect -p linux/arm/v7 ghcr.io/nilcons/debian
docker run -it --rm ghcr.io/regclient/regctl image inspect -p linux/arm/v7 docker.io/nilcons/debian
```
