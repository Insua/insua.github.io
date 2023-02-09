---
title: Laravel 执行定时任务时的url域名
date: 2018-01-30 22:22:30
tags:
- laravel
---

在laravel开发时，我最常用的url函数是route，因为根据route函数生成的url如果命名路由写错了那么直接会报错，这样有问题能早发现。
在生成定时任务的时候，在console里，我也写了route，但是推送到用户的地址却是localhost，这可太奇怪了，于是翻了laravel的源代码

>laravel/framework/src/Illuminate/Foundation/Bootstrap/SetRequestForConsole.php

```php
 public function bootstrap(Application $app)
    {
        $url = $app->make('config')->get('app.url', 'http://localhost');

        $app->instance('request', Request::create($url, 'GET', [], [], [], $_SERVER));
    }
```

原来是因为走定时器任务时是在命令行里执行的，laravel无法根据用户的输入而得到域名，那么就必须读取配置文件了
当然在 `.env`里定义是最佳实践了
