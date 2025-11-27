# epicsmng docker images

This image has been developed to be used as base images for other projects.

The base image is the minimal version of almalinux. This image does not have `dnf`; instead use `microdnf`.

It provides the `epicsmng` executable; it uses the non-root `epics` user and set the epics user's home (`/home/epics`) as the workspace directory.

## How to build images

This image has to be built with the main repository folder as context:

```bash
docker build -t epicsmng -f docker/Dockerfile .
```
