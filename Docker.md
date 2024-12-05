
# Docker Cheatsheet

## Overview

Docker consists of a daemon server and a CLI (to talk to the server.) An image is like a blueprint, and a container is a running image (similar to a virtual machine.) The image definition is created by defining specifications in a file named Dockerfile.

A container consists of code, dependencies, and a configuration. Building a container generally involves:

- Adding code, workflow, files, etc
- Installing dependencies
- Configuration (ip addresses, etc)

## Installation

- On Ubuntu, docker-related data (images, etc) is stored by default in /var/lib/docker

## Container administration

### Show available local images

`$ docker images`

### Run a container

~~~
# General format:
$ docker run <image-name>

# Examples:
$ docker run hello-world
$ docker run ubuntu
~~~

### Run a container and return an interactive shell within the started container

~~~
# General format:
$ docker run -it <image-name>

# Example:
$ docker run -it ubuntu
# Show the ip address of the running container
/# hostname -i
# Exit the interactive shell of the container
/# exit
$
~~~

### Run a container in the background

~~~
# General format:
$ docker run -d <image-name>

# Example (with passing args to container):
$ docker run -de POSTGRES_PASSWORD=123456 postgres
# Show running containers
$ docker ps
# Stop running container
$ docker stop <container-id>
~~~

### Run a container with a name (which can be used in other commands in place of the id)

~~~
# General format:
$ docker run --name <container-name> <image-name>

# Example
$ docker run --name postgres -de POSTGRES_PASSWORD=123456 postgres
# Stop it by name
$ docker stop postgres
# Start it again by name
$ docker start postgres
~~~

### List running containers

`$ docker ps`

### List running containers filtered by name

`$ docker ps -f "name=<container-name>"`

### Stop a running container

`$ docker stop <container-id>`

### Delete a container

`$ docker container rm <container-id>`

### Show container output

~~~
# General format:
$ docker logs <container-id>

# Example:
$ docker run --name postgres -de POSTGRES_PASSWORD=123456 postgres
$ docker logs postgres
# View (follow) logs in realtime
$ docker logs -f postgres
# Exit live log view
$ CTRL-C
~~~

### Show container output in real-time. Ctrl-C to exit

`$ docker logs -f <container-id>`

### Pull an image down from [dockerhub.com](https://hub.docker.com/)

~~~
# General format:
$ docker pull <image-name>

# Examples:
$ docker pull hello-world
$ docker pull ubuntu
# Pull specific version
$ docker pull ubuntu:22.04
# Pull specific version by tag
$ docker pull ubuntu:jammy
# Show all containers, including stopped ones
$ docker ps -a
# Remove all stopped containers
$ docker container prune
# Delete a local image (only possible when there are no containers that are based on it)
$ docker image rm hello-world
# Delete all dangling images (an image that is not tagged or referenced by any container)
$ docker image prune
# Delete all images without at least one container associated to them
$ docker image prune -a
~~~

### Pull a specific image version (syntax of version specification works with other commands)

`$ docker pull <image-name>:<image-version>`

### Pull from a private registry (anything other than docker hub)

`$ docker pull <registry-url>/<image-name>`

### Push an image to docker hub

`$ docker image push <image-name>`

### Push to a private registry

`$ docker image push <registry-url>/<image-name>`

### Rename an image

`$ docker tag <old-image-name> <new-image-name>`

### Remove an image (can't be used by existing containers)

`$ docker image rm <image-name>`

### Remove all stopped containers

`$ docker container prune`

### Remove all unused images

`$ docker image prune -a`

### Log into a registry

`$ docker login <login-url>`

### Save an image as a file

~~~
# General format:
$ docker save -o <filename.tar> <image-name>`

# Example:
$ docker save -o postgres.tar postgres
~~~

### Load an image from a file

`$ docker load -i <filename.tar>`

## Creating images

An image specification is stored in a file called Dockerfile. Here is an example:

~~~

# An image starts by specifying a base image.
FROM ubuntu

# Followed by build commands (any valid shell commands).
# Commands can be chained together via '&&'.
# Commands can span multiple lines by suffixing them a '\'.

RUN apt-get update
RUN apt-get install -y python3

# Copy a local host file to a destination on the image
COPY <source-path-on-host> <destination-path-on-image>

# Copy from remote url
RUN curl <remote-url> -o <destination-file>.zip
RUN unzip <destination-file>.zip
RUN rm <destination-file>.zip

# Set the working directory for all following instructions
WORKDIR <path-to-working-directory>

# Set the user for all following instructions
USER <user-name>

# Use a variable. Not accessible after image is built
ARG <var-name>=<var-values>

# Variable example
ARG path=/home/smith
COPY /local/path $path

# Use an environment variable. Accessible after image is built
ENV <var-name>=<var-values>

~~~

### Build an image

`$ docker build <dockerfile-path>`

### Build an image with a specified name

`$ docker build -t <image-name> <dockerfile-path>`

### Specify a custom argument from the command line

`$ docker build --build-arg <arg-name>=<arg-values>`

### Set or replace an environment variable from the command line

`$ docker run --env <key>=<value> <image-name>`

### Show instructions used to create an image

`$ docker history <image-name>`
