# Docker Master Class

This repository is a todo application for the Docker Master Class.

The application is based on the application from the Getting Started tutorial at https://github.com/docker/getting-started

---

- [Docker Master Class](#docker-master-class)
  - [Task 00 - Demonstration of Working Linux Namespaces and Cgroups](#task-00---demonstration-of-working-linux-namespaces-and-cgroups)
    - [Create basic docker image](#create-basic-docker-image)
    - [Run sleep-alpine container](#run-sleep-alpine-container)
    - [Connect to sleep-alpine container](#connect-to-sleep-alpine-container)
    - [Display namespaces](#display-namespaces)
    - [Connect to alpine-sleep namespaces](#connect-to-alpine-sleep-namespaces)
    - [Check container\_id](#check-container_id)
    - [Go to alpine-sleep container cgpoup folder](#go-to-alpine-sleep-container-cgpoup-folder)
    - [Check alpine-sleep container Memory limit in Mb](#check-alpine-sleep-container-memory-limit-in-mb)
    - [Check alpine-sleep container CPU limit](#check-alpine-sleep-container-cpu-limit)
  - [Task 01 - Build an Image Based on Ubuntu](#task-01---build-an-image-based-on-ubuntu)
    - [Build image](#build-image)
    - [Run container](#run-container)
    - [Cleanup](#cleanup)
  - [Task 02 - Build an Image Based on Ubuntu with Optimized Layers](#task-02---build-an-image-based-on-ubuntu-with-optimized-layers)
    - [Build image](#build-image-1)
    - [Run container](#run-container-1)
    - [Cleanup](#cleanup-1)
  - [Task 03 - Build an Image Based on Ubuntu with Optimized Layers and Cleanup Cache](#task-03---build-an-image-based-on-ubuntu-with-optimized-layers-and-cleanup-cache)
    - [Build image](#build-image-2)
    - [Run container](#run-container-2)
    - [Cleanup](#cleanup-2)
  - [Task 04 - Build an Image on Node Alpine](#task-04---build-an-image-on-node-alpine)
    - [Build image](#build-image-3)
    - [Run container](#run-container-3)
    - [Cleanup](#cleanup-3)
  - [Task 05 - Build an Image on Node Alpine with Optimized Layers and Security Enhancements](#task-05---build-an-image-on-node-alpine-with-optimized-layers-and-security-enhancements)
    - [Build image](#build-image-4)
    - [Run container](#run-container-4)
    - [Cleanup](#cleanup-4)
  - [Task 06 - Multi-Stage Build with Docker Volume Demonstration](#task-06---multi-stage-build-with-docker-volume-demonstration)
    - [Build image](#build-image-5)
    - [Create docker volume](#create-docker-volume)
    - [Run container with volume](#run-container-with-volume)
    - [Cleanup](#cleanup-5)
  - [Task 07 - Docker Compose Demonstration](#task-07---docker-compose-demonstration)
    - [Create file with MySQL credentials](#create-file-with-mysql-credentials)
    - [Run docker compose](#run-docker-compose)
    - [Cleanup](#cleanup-6)

---

## Task 00 - Demonstration of Working Linux Namespaces and Cgroups

### Create basic docker image

```sh
docker build -t sleep-alpine  -<<EOF
FROM alpine
ENTRYPOINT ["sleep"]
CMD ["3600"]
EOF
```

### Run sleep-alpine container

```sh
docker run --name sleep-alpine --memory=100m --cpus=".2" -d sleep-alpine
```

### Connect to sleep-alpine container

```sh
docker exec -it sleep-alpine /bin/sh
```

### Display namespaces

```sh
sudo lsns
```

### Connect to alpine-sleep namespaces

```sh
sudo nsenter -t <container_pid> --pid --net --mount --ipc --uts sh
```

### Check container_id

```sh
sudo systemd-cgls
```

### Go to alpine-sleep container cgpoup folder

```sh
cd /sys/fs/cgroup/system.slice/docker-<container_id>
```

### Check alpine-sleep container Memory limit in Mb

```sh
cat memory.max | awk '{$1=$1/1024/1024; print}'
```

### Check alpine-sleep container CPU limit

```sh
cat cpu.max
```
---

## Task 01 - Build an Image Based on Ubuntu

### Build image

```sh
docker build --no-cache -t todos-app-task-01 -f task01/Dockerfile .
```

### Run container

```sh
docker run -dp 3000:3000 --name todos-app-task-01 todos-app-task-01
```

### Cleanup

```sh
docker stop todos-app-task-01 && docker rm todos-app-task-01
```

---

## Task 02 - Build an Image Based on Ubuntu with Optimized Layers

### Build image

```sh
docker build --no-cache -t todos-app-task-02 -f task02/Dockerfile .
```

### Run container

```sh
docker run -dp 3000:3000 --name todos-app-task-02 todos-app-task-02
```

### Cleanup

```sh
docker stop todos-app-task-02 && docker rm todos-app-task-02
```

---

## Task 03 - Build an Image Based on Ubuntu with Optimized Layers and Cleanup Cache

### Build image

```sh
docker build --no-cache -t todos-app-task-03 -f task03/Dockerfile .
```

### Run container

```sh
docker run -dp 3000:3000 --name todos-app-task-03 todos-app-task-03
```

### Cleanup

```sh
docker stop todos-app-task-03 && docker rm todos-app-task-03
```

---

## Task 04 - Build an Image on Node Alpine

### Build image

```sh
docker build --no-cache -t todos-app-task-04 -f task04/Dockerfile .
```

### Run container

```sh
docker run -dp 3000:3000 --name todos-app-task-04 todos-app-task-04
```

### Cleanup

```sh
docker stop todos-app-task-04 && docker rm todos-app-task-04
```

---

## Task 05 - Build an Image on Node Alpine with Optimized Layers and Security Enhancements

### Build image

```sh
docker build --no-cache -t todos-app-task-05 -f task05/Dockerfile .
```

### Run container

```sh
docker run -dp 3000:3000 --name todos-app-task-05 todos-app-task-05
```

### Cleanup

```sh
docker stop todos-app-task-05 && docker rm todos-app-task-05
```

---

## Task 06 - Multi-Stage Build with Docker Volume Demonstration

### Build image

```sh
docker build --no-cache -t todos-app-task-06 -f task06/Dockerfile .
```

### Create docker volume

```sh
docker volume create todo-db
```

### Run container with volume

```sh
docker run -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos --name todos-app-task-06 todos-app-task-06
```

### Cleanup

```sh
docker stop todos-app-task-06 && docker rm todos-app-task-06 && docker volume rm todo-db
```

---

## Task 07 - Docker Compose Demonstration

### Create file with MySQL credentials

> [!NOTE]
> Change the MYSQL_PASSWORD to your own

```sh
cat << EOF > .env
MYSQL_HOST=mysql
MYSQL_USER=root
MYSQL_PASSWORD=<DBPassword>
MYSQL_DB=todos
EOF
```

### Run docker compose

```sh
docker-compose up -d
```

### Cleanup

```sh
docker-compose down
```