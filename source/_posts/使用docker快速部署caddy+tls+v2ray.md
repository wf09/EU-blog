---
title: 使用docker快速部署caddy+tls+v2ray
date: 2020/8/28 20:00:00
tags:
  - Docker
  - 代理
abbrlink: d125f563
cover: https://ftp.fly97.cn/image/docker.jpeg
---

### 安装docker

执行以下命令：

```sh
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror 
# sudo sh get-docker.sh --mirror Aliyun

```

### 启动Docker-CE

```sh
sudo systemctl enable docker
sudo systemctl start docker
```

### 测试 Docker 是否安装正确

```sh
docker run hello-world
以下是输出：
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 部署混合镜像

取回配置文件

```sh
git clone git@e.coding.net:fly97/docker/docker.git
```

切换到

```sh
cd docker-v2ray
```

构建镜像

```bash
docker build -t v2fly .
```

运行容器

```sh
docker run --name v2fly --restart=always -d -v -p 80:80 -p 443:443 -v /etc/v2ray:/etc/v2ray -e METHOD=tls -e DOMAIN=your.domain v2fly
```

**说明：**

1. METHOD=**socks5**时，v2ray使用**ws + socks5**做后端，且使用SSL加密。
2. METHOD=**tls**时，v2ray使用**ws+vmess**做后端，且使用SSL加密。
3. METHOD=**http**时，v2ray使用**ws+vmess**做后端，且不使用SSL加密。
4. DOMAIN的值是VPS绑定的域名。

注意：运行前请先查看**80和443端口**是否被占用。