---
title: 在Arch上使用Laravel valet
date: 2017-05-13 10:22:57
tags:
- Archlinux
- Laravel
- valet
categories: Laravel  
---
valet-linux的github[地址](https://github.com/cpriego/valet-linux)  
valet默认是使用NetWorkManager来管理dnsmasq的，不过我是用netctl来管理网络的，所以默认安装后ping foo.app并不能指向127.0.0.1  
那么就直接用dnsmasq了  
```bash
$ sudo vim /etc/dnsmasq.conf 
```
  
修改listen-address和address
  
```bash
listen-address=127.0.0.1  
  
address=/app/127.0.0.1
address=/dev/192.168.10.10
```
 
> 指向dev的是homestead的ip,valet的默认domain修改为app  

修改 /etc/resolv.conf
将

```bash
nameserver 127.0.0.1
```

加到最上面  
使用

```bash
$ sudo chattr +i /etc/resolv.conf 
```

为其加锁，最后别忘了用systemd将dnsmasq自动启动

```bash
$ sudo systemctl enable dnsmasq
```

valet-linux其他的使用按照github wiki上的操作即可，目前没有发现什么异常
