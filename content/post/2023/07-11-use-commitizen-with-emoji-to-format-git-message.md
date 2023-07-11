---
title: "Use Commitizen With Emoji to Format Git Message"
date: 2023-07-11T10:55:49+08:00
tags:
  - git
---
Global install commitizen with npm  
```shell
npm install -g commitizen
```
Global install cz-emoji  
```shell
npm install -g cz-emoji
```
Set cz-emoji as default adapter
```shell
echo '{ "path": "cz-emoji" }' > ~/.czrc
```
Use **git cz** instead of **git commit** to edit git commit message
