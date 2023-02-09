---
title: 开发Laravel时必备的三个包
date: 2017-01-17 21:16:46
tags:
- laravel
- php
- package
categories: php
---
### barryvdh/laravel-debugbar
作用是页面底部显示丰富的调试信息
>安装

```bash  
$ composer require barryvdh/laravel-debugbar
```
>config/app.php注册ServerProvider

```php
Barryvdh\Debugbar\ServiceProvider::class,
```
>增加Facades

```php
'Debugbar' => Barryvdh\Debugbar\Facade::class,
```
### barryvdh/laravel-ide-helper
生成一个php文件，为IDE提供更好的支持
>安装

```bash 
$ composer require barryvdh/laravel-ide-helper
```
>config/app.php注册ServerProvider

```php
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
```
>使用

如果使用的是Homestead，ssh登录虚拟机后，在虚拟机执行，否则会找不到数据库而报错
### sven/artisan-view
使用artisan命令生成视图文件
>安装

```bash 
$ composer require sven/artisan-view
```
>config/app.php注册ServerProvider

```php
Sven\ArtisanView\ArtisanViewServiceProvider::class,
```
>使用

生成视图
```bash
$ php artisan make:view index
```
生成目录下的视图
```bash
$ php artisan make:view pages.index
```
