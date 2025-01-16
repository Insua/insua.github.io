---
title: "arch 信任签名根CA"
date: 2025-01-16T17:51:38+08:00
tags:
  - arch
---
下载根证书，然后转换
```shell
openssl pkcs12 -in baota_root.pfx -out baota_root.pem -nodes
```
输入根证书密码
```shell
openssl x509 -in baota_root.pem -out baota_root.crt
```
把crt的名字加上服务器的ip来做区分
```shell
mv baota_root.crt baota_root_192.168.1.1.crt
```
移动到系统目录
```shell
sudo mv baota_root_192.168.1.1.crt /etc/ca-certificates/trust-source/anchors/
```

更新系统证书存储
```shell
sudo trust extract-compat
```
重启浏览器，然后进入宝塔面板，就会发下证书已被信任了
