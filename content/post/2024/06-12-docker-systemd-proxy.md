---
title: "Docker Systemd Proxy"
date: 2024-06-12T15:34:27+08:00
tags:
  - docker
---
当前 docker 的镜像源全部都挂了.  
目前唯一的方法就是使用代理来拉取镜像.  
```shell
vim /etc/systemd/system/docker.service.d/http-proxy.conf
```
然后写入  

```
[Service]
Environment="HTTP_PROXY=socks5://127.0.0.1:7890/"
Environment="HTTPS_PROXY=socks5://127.0.0.1:7890/"
Environment="NO_PROXY=localhost,127.0.0.1"
```
之后重启 docker
```shell
systemctl daemon-reload
systemctl restart docker
```

之后无论是 docker pull 还是 docker-compose build 就都可以通过代理拉取镜像了.
