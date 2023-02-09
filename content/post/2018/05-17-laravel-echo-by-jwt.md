---
title: laravel echo server 结合 jwt 授权
date: 2018-05-17 19:59:24
tags:
- laravel
- websocket
---
首先，因为重写了 axios 的拦截器，为了确保 socket 消息头的引入，在 axios 拦截器中中加入
```javascript
if(window.Echo)
  {
    let socketId = window.Echo.socketId()
    if (socketId)
    {
      request.headers.common['X-Socket-Id'] = socketId
    }
  }
```

然后，修改BroadcastServiceprovider，修改认证路由为api模组
```php
 Broadcast::routes(["prefix" => "api", "middleware" => "api"]);
```

最后，在 laravel-echo-server.json 中确保认证路由的正确
```
{
	"authHost": "http://dev.test",
	"authEndpoint": "/api/broadcasting/auth"
}
```
同时，在 Laravel Echo中加入token认证
```javascript
import Cookies from 'js-cookie'

let token = Cookies.get('token')
window.Echo = new Echo({
  broadcaster: 'socket.io',
  host: process.env.MIX_ECHO_SERVER,
  auth: { headers: { 'Authorization': 'Bearer ' + token } }
})
```

好了，我们现在可以愉快的使用 laravel 发送广播了，私有频道在 routes/channels.php 中编写认证权限  

在前端使用 Laravel Echo 监听私有频道时，只有通过 laravel echo server 认证的私有频道才会建立监听，并收听到广播
