# Docker Master Class

This repository is a todo application for the Docker Master Class.

The application is based on the application from the Getting Started tutorial at https://github.com/docker/getting-started

# Tasks list

- [Task 00 - Demonstration of Working Linux Namespaces and Cgroups](#Task-00)
- [Task 01 - Build an Image Based on Ubuntu](#Task-01)
- [Task 02 - Build an Image Based on Ubuntu with Optimized Layers](#Task-02)
- [Task 03 - Build an Image Based on Ubuntu with Optimized Layers and Cleanup Cache](#Task-03)
- [Task 04 - Build an Image on Node Alpine](#Task-04)
- [Task 05 - Build an Image on Node Alpine with Optimized Layers and Security Enhancements](#Task-05)
- [Task 06 - Multi-Stage Build with Docker Volume Demonstration](#Task-06)
- [Task 07 - Docker Compose Demonstration](#Task-07)

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

## Task 06 - Multi-Stage Build with Docker Volume Demonstration

### Build image
```sh
docker build --no-cache -t todos-app-task-06 -f task06/Dockerfile .
```

### Run container
```sh
docker run -dp 3000:3000 --name todos-app-task-06 todos-app-task-06
```

### Cleanup
```sh
docker stop todos-app-task-06 && docker rm todos-app-task-06
```

## Task 07 - Docker Compose Demonstration

### Run docker compose
```sh
docker-compose up -d
```

### Cleanup
```sh
docker-compose down
```