---
title: image-webpack-loader resolve jpg file wrong in ubuntu server
date: 2018-08-02 10:35:40
tags:
- webpack
- ubuntu
---

In my production Ubuntu server which version is 16.04.3 LTS,after push my code to it,I run
```
yarn prod
```
but it show me a error notice
```
ERROR in ./resources/assets/js/mobile/pages/Home/images/banner-oath.jpg
Module build failed: Error: write EPIPE
    at _errnoException (util.js:992:11)
    at WriteWrap.afterWrite [as oncomplete] (net.js:864:14)

```
after search by google,this [link](https://github.com/tcoopman/image-webpack-loader/issues/142) solve my problem  
install libpng16-dev in server
```
sudo apt-get install libpng16-dev
```
then, it is all fine.
