---
title: 使用Route macro来定义Route的新方法
date: 2018-11-06 17:43:17
tags:
- laravel
---
使用 Laravel 在做后台表单操作时，通常会增加一个批量删除的功能
起初是在 **route** 里定义一个新的 delete
```
Route::delete('prefix/destroy-selection', 'CurrentController@destroySelection);
```
之后再定义 apiResource  
```
Route::apiResource('prefix', 'CurrentController');
```
这样写十分啰嗦且冗长，Laravel 的强大之处就是它提供了 Macro 来扩展功能  

在 AppServiceProvider 的 boot 方法里加入 
```
Route::macro('full',function ($prefix, $controller){
            Route::delete($prefix.'/destroy-selection', $controller.'@destroySelection')->name($prefix.'destroy-selection');
            Route::apiResource($prefix, $controller);
});
```
之后在 **route** 里就可以直接使用 full 来定义了   
```
 Route::full('prefix', 'CurrentController');
```

