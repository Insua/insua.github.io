---
title: Laravel Elixir使用webpack打包只引入moment.js的中文语言包
date: 2018-02-08 09:21:58
tags:
- laravel
- webpack
- moment.js
---

方法是使用webpack内置的ContextReplacementPlugin插件  
修改_gulpfile.js_  
先引入webpack
```javascript
const webpack = require('webpack')
```
增加自定义webpack配置
```javascript
elixir(mix => {

    Elixir.webpack.mergeConfig({
        plugins: [
            new webpack.ContextReplacementPlugin(/moment[\\/]locale$/, /^\.\/(zh-cn)$/)
        ]
    });
```
之后运行
```bash
gulp
```
会发现打包之后的js会明显变小

