---
title: AUR打包成pkg
date: 2017-02-05 20:40:15
tags:
- Linux
- Arch Linux
- AUR
categories: Linux
---
大多数情况下，我们都是使用yaourt等AUR包管理工具来安装、更新AUR包，不过有些时候在网络条件比较差的情况下，源文件下载特别慢而build失败，我们先把PKGBUILD下载到本地
```bash
$ yaourt -G pkg-name
$ cd pkg-name
```
查看PKGBUILD
```bash
$ vim PKGBUILD
```
在找到源码的地址后，可以使用下载工具将源码下载或克隆到本地，修改源码地址，之后编译生成pkg包
```bash
$ makepkg
```
编译、打包完成后，在目录下会生成pkg-name-version-arch.pkg.tar.xz   
直接安装到本机的话，执行命令
```bash
$ sudo pacman -U pkg-name-version-arch.pkg.tar.xz
```
