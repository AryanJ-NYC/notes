# Dockerfile

## FROM

The first line of a Dockerfile is usually the  `FROM` keyword followed by a base image name. Every other instruction in the Dockerfile following the  `FROM` instruction will create a new image that blends together everything in the base **plus** the modifications we're making in the rest of the Dockerfile.

```text
FROM httpd:2.4
```

## EXPOSE

Next, we can use the Dockerfile to expose a port inside of its associated container. Note that we'll still need to map ports between the host and container when we run  `docker container run`, but this line will let the image know which ports inside of the container should become available.

```text
EXPOSE 80
```

## RUN

Used to run commands as the image is being built.

```text
RUN apt-get update
RUN apt-get install -y fortunes
```

## COPY

Used to copy a file into a container.

```bash
# COPY <local_file_path> <container_path>

COPY page.html /usr/local/apache2/htdocs/
```

## LABEL

Label accepts key-value pairs in the form of `key=“value"`

```bash
LABEL maintainer="moby-dock@example.com"
```

## CMD

Runs a set of commands.

## Building an Image from a Dockerfile

The `docker image build` command builds a new image.  The `—tag` option is a useful option to pass to `docker build` as it specifies the name of the new image followed by a version number.

```bash
# docker image build --tag web-server:1.0 <path>

docker image build --tag web-server:1.0 .
```

