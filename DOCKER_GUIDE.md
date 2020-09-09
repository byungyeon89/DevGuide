# Docker Guide

## Install Docker Engine on Ubuntu

### Install Docker Engine on Ubuntu
1. Uninstall old versions
Older versions of Docker were called `docker`, `docker.io`, or `docker-engine`. **If these are installed, uninstall them:**
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```
The contents of `/var/lib/docker/`, including images, containers, volumes, and networks, are preserved. The Docker Engine package is now called `docker-ce`.

2. Install using the convenience script
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```
If you would like to use Docker as a non-root user, you should now consider adding your user to the `docker` group with something like:
```
$ sudo usermod -aG docker your-user
```

## Install Nvidia-Docker Engine on Ubuntu

**If you wish to use GPU with Docker use nvidia-docker** to run your image instead of regular docker.

1. Uninstall old versions
```
$ docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
$ sudo apt-get purge -y nvidia-docker
```
2. Add the package repositories
```
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
    sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt-get update
```

3. Install nvidia-docker2 and reload the Docker daemon configuration
```
$ sudo apt-get install -y nvidia-docker2
$ sudo pkill -SIGHUP dockerd
```

4. Test nvidia-smi with the latest official CUDA image
```
$ docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
$ nvidia-docker run --rm nvidia/cuda:9.0-base nvidia-smi
```

## Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, and Containerd packages:
```
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io
```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:
```
$ sudo rm -rf /var/lib/docker
```
You must delete any edited configuration files manually.

