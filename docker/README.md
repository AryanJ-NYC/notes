---
description: 'https://devopswithdocker.com/'
---

# Docker

## Pull an Image or Repository

```bash
docker pull <docker-image>
```

## Build a Container

```bash
docker build -t <container-name> <path>

#e.g.
docker build -t smuush .
```

## [Tag](https://docs.docker.com/engine/reference/commandline/tag/) Images

```bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

#e.g.
docker tag hello-world aws_account_id.dkr.ecr.us-east-1.amazonaws.com/hello-world
```

## Share Image

Use [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) to share your images to the [Docker Hub](https://hub.docker.com) registry or to a self-hosted one.

```bash
docker push [OPTIONS] NAME[:TAG]

# e.g.
docker tag rhel-httpd registry-host:5000/myadmin/rhel-httpd
docker push registry-host:5000/myadmin/rhel-httpd
```

## Mapping Port

The `-p` flag means publish ports. The first number is the port on the host we want to open up and the second number is the port in the container we want to map it to.

```bash
# Port 80 is a standard port for handling web requests but we could map any port on the host to the container.

docker container run -p 9999:80 httpd:2.4
```

## Attaching a Shell to a Container

The `-i` and `-t` flags can be passed to the `exec` command to keep an interactive shell open. The shell you want to attach is specified after the container ID.

```bash
 docker container exec -it <container_name> /bin/bash
```

## Copying Files into a Running Container

```bash
 docker container cp <local_file_path> <container_name>:<container_path>
 
 # e.g.
  docker container cp page.html elegant_noether:/usr/local/apache2/htdocs/
```

## List Images

```bash
docker image ls
```

## Data Volume

Data volumes expose files on your host machine to the container.

```bash
 docker run -d -p 80:80 -v /my-files:/usr/local/apache2/htdocs web-server:1.1
```

