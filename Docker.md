---
title: 'Docker'
date: '2022-08-14 22:59:00'
tags:
  - docker
  - linux
public: true
---

# Docker

## Image vs Container

- **container** is a running env for **image**
  - it provides:
    - application image
      - postgres, redis
    - file system (virtual)
    - env configs
  - it has **port** to connect to the application that runs inside the container
    - for example: 5000


## Basic commands

### docker pull

```bash
docker pull <image>
```

- it pulls an image from the registry (by default: [Docker Hub](https://hub.docker.com/))

### docker images

```bash
docker images
```

- returns list of downloaded/available images on your machine

### docker run

```bash
docker run <image>
```

- it runs/start the image in the container

**flags**

- "-d" --> runs the image in the container in detached mode (smth like as a daemon), it also returns an id of started container


### docker ps

```bash
docker ps
```

- returns list of all running containers
- the list contains ids of running containers
  - these ids could be used in other commands like `docker stop <id>` or `docker start <id>`

**flags**

- "-a" --> returns list of *running* and *stopped* containers

### docker stop

```bash
docker stop <container_id>
```

- stops the container with id = <container_id>

### docker start

```bash
docker start <container_id>
```

- start the container with id = <container_id>



