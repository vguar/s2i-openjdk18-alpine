
# s2i-openjdk18-alpine

This repository contains the sources and [Dockerfile](https://github.com/vguar/s2i-openjdk18-alpine/blob/master/Dockerfile) of the base image for deploying Spring Boot applications as reproducible Docker images.
The resulting images can be run either by [Docker](http://docker.io) or using [S2I](https://github.com/openshift/source-to-image).

This image is heavily inspired by the  [sclorg/s2i-nodejs-container](https://github.com/https://github.com/sclorg/s2i-nodejs-container) builder images.

This image is based on Alpine.

## Usage

To build a simple s2i-openjdk18-alpine application using standalone S2I and then run the resulting image with Docker execute:



```sh
$ s2i build git://github.com/vguar/hello-world-java.git vguar/s2i-openjdk18-alpine sample-app
$ docker run -p 8080:8080 sample-app
```

**Accessing the application:**

```
$ curl 127.0.0.1:8080
```

## Repository organization

* **`s2i/bin/`**

  This folder contains scripts that are run by [S2I](https://github.com/openshift/source-to-image):

  *   **assemble**

      Is used to restore the build artifacts from the previous built (in case of
      'incremental build'), to install the sources into location from where the
      application will be run and prepare the application for deployment (eg.
      using maven to build the application etc..)

  *   **run**

      This script is responsible for running a Spring Boot fat jar using `java -jar`.
      The image exposes port 8080, so it expects application to listen on port
      8080 for incoming request.

  *   **save-artifacts**

      In order to do an *incremental build* (iow. re-use the build artifacts
      from an already built image in a new image), this script is responsible for
      archiving those. In this image, this script will archive the
      `/opt/java/.m2` directory.

## Build Image


## Environment variables

*  **MVN_ARGS** (default: '')

    This variable specifies the arguments for Maven inside the container.

## Using the template provided in this repo.
```sh
oc import-image --from=vguar/s2i-openjdk18-alpine openshift/s2i-openjdk18-alpine -n openshift --confirm
oc new-build --strategy=docker --name=sample-app-jdk8 https://git://github.com/vguar/hello-world-java.git -n myproject
```

## Copyright

Released under the Apache License 2.0. See the [LICENSE](https://github.com/codecentric/springboot-maven3-centos/blob/master/LICENSE) file.
