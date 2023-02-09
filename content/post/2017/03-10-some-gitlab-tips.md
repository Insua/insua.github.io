---
title: 一些gitlab的tips
date: 2017-03-10 20:53:45
tags:
- git
- gitlab
categories: git
---
Tips1: 禁止登录  
登录服务器修改数据库  
```bash
$ sudo -u gitlab-psql /opt/gitlab/embedded/bin/psql -h /var/opt/gitlab/postgresql gitlabhq_production    
update application_settings set signin_enabled=true;
```
Tips2: 忘记密码
登录服务器，进入console修改  
```bash
$ sudo gitlab-rails console production  
user = User.find(1)  
user.password = "xxxxx"  
user.save
```
