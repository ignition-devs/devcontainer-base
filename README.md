# ignition-devs devcontainer base images

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/ignition-devs/devcontainer-base/main.svg)](https://results.pre-commit.ci/latest/github/ignition-devs/devcontainer-base/main)

Base images for our development workspace needs.

## Supported tags

- [jython](https://github.com/ignition-devs/devcontainer-base/blob/main/jython/Dockerfile)
- [python(https://github.com/ignition-devs/devcontainer-base/blob/main/python/Dockerfile)]

## How to use this image

On your devcontainer Dockerfile add this line:

```dockerfile
FROM quay.io/ignition-devs/devcontainer-base:<tag>
...
```
