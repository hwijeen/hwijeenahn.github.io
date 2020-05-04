---
layout: post
title: "Docker cheat sheet"
description: "자주 사용하는 명령어"
comments: true
categories: [Cheatsheet]
tags:
- Docker
- Cheat sheet
---

## Resources

- Basics: [Slides from Nvidia](https://www.nvidia.co.kr/content/apac/event/kr/deep-learning-day-2017/dli-1/Docker-User-Guide-17-08_v1_NOV01_Joshpark.pdf) / [한국어 블로그](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
- 사전처럼 사용할 수 있는 [블로그](http://pyrasis.com/docker.html) or [official docker docs](https://docs.docker.com/)



## Prerequisites

- install [docker]([https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html#%EB%8F%84%EC%BB%A4-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html#도커-설치하기))
- install [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)
```bash
sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한주기
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi # check if nvidia-docker is installed
docker info
docker stats
```



## Images

```bash
docker search (IMAGE_KEYWORD) # search from dockerhub
docker pull $IMAGE_NAME

docker images # list images
docker rmi $IMAGE_NAME

docker build -t (IMAGE_NAME) . # build image from dockerfile
```



## Containers

```bash
docker ps # list containers
docker run -it -v /home/host:/home/container -p 8888:8888 --name (CONTAINER) -d -e HOME:$HOME $IMAGE 
docker start $CONTAINER # start stopped container
docker attach $CONTAINER # get into the container
docker rm $CONTAINER # warning - this deletes file!
docker commit -a '(USERNAME) (<EMAIL>)' -m '(MESSAGE)' $CONTAINER (IMAGENAME):(TAG) # from container to image
```



## Dockerfile(example)

- use `.dockerignore `
- consider [build cache](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)

```dockerfile
FROM nvidia/cuda:10.0-base-ubuntu16.04
LABEL maintainer "NVIDIA CORPORATION <cudatools@nvidia.com>"

ENV NCCL_VERSION 2.4.2

RUN apt-get update && apt-get install -y --no-install-recommends \
        cuda-libraries-$CUDA_PKG_VERSION \
        cuda-nvtx-$CUDA_PKG_VERSION \
        libnccl2=$NCCL_VERSION-1+cuda10.0 && \
    apt-mark hold libnccl2 && \
    rm -rf /var/lib/apt/lists/*


# from kaixhin/torch/dockerfile
RUN apt-get update && apt-get install -y \
        git \
        software-properties-common \
        libssl-dev \
        libzmq3-dev \
        python-dev \
        python-pip \
        python-zmq \
        sudo

RUN git clone https://github.com/torch/distro.git /root/torch --recursive && cd /root/torch && \
    bash install-deps && \
    ./install.sh

ENV LUA_PATH='/root/.luarocks/share/lua/5.1/?.lua;/root/.luarocks/share/lua/5.1/?/init.lua;/root/torch/install/share/lua/5.1/?.lua;/root/torch/install/share/lua/5.1/?/init.lua;./?.lua;/root/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua'
ENV LUA_CPATH='/root/.luarocks/lib/lua/5.1/?.so;/root/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so'
ENV PATH=/root/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/root/torch/install/lib:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH=/root/torch/install/lib:$DYLD_LIBRARY_PATH
ENV LUA_CPATH='/root/torch/install/lib/?.so;'$LUA_CPATH


## added by hwijeen
RUN apt-get install -y \
        vim \
        gcc-4.8-plugin-de \
        wget
RUN mkdir -p /home/Downloads
WORKDIR /home/Downloads
RUN curl -R -O http://www.lua.org/ftp/lua-5.3.5.tar.gz && \
    tar zxf lua-5.3.5.tar.gz && \
    cd lua-5.3.5 && \
    make linux test && \
    sudo make install
WORKDIR /home/Downloads
RUN wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.20/src/hdf5-1.8.20.tar.gz &&\
    tar -zxvf hdf5-1.8.20.tar.gz && \
    cd hdf5-1.8.20 && \
    make install
ENV PATH=/home/Downloads/hdf5-1.8.20/hdf5:$PATH
RUN luarocks install hdf5 20-0
RUN mkdir -p /home/WNGT2019
WORKDIR /home/WNGT2019
COPY . ./
```





## Temporary shell file(example)

- make shell before writing dockerfile

```bash
sudo apt-get update
sudo apt-get install vim
sudo apt-get install gcc-4.8-plugin-de
sudo apt-get install wget

curl -R -O http://www.lua.org/ftp/lua-5.3.5.tar.gz # install lua
tar zxf lua-5.3.5.tar.gz
cd lua-5.3.5
make linux test
sudo make install

wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.20/src/hdf5-1.8.20.tar.gz
tar -zxvf hdf5-1.8.20.tar.gz
cd hdf5-1.8.20
make install

luarocks install hdf5 20-0
```


