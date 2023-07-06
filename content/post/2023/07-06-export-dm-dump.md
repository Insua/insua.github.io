---
title: "命令行导出达梦备份文件"
date: 2023-07-06T17:50:40+08:00
tags:
  - dameng
---
必须进入到达梦的 bin 目录下执行 否则会找不到 so 文件  
命令如下:  
```shell
./dexp USERID=user/pass@ip SCHEMAS=schema DIRECTORY=dir
```
参数解释:  
- user: 用户
- pass: 密码
- schema: 模式
- dir: 导出目录
