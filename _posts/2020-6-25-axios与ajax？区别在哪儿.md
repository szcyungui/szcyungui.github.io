---
layout:     post
title:   axios与ajax?
date:       2020-06-25
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: 
    - Vue
    - React
    - 函数式编程
    - ES6
---
  
## 关系

axios是通过promise实现对ajax技术的一种封装，就像jQuery实现ajax封装一样。

简单来说： ajax技术实现了网页的局部数据刷新，axios实现了对ajax的封装。

axios是ajax ajax不止axios。

下面列出代码来对比一下：

```
axios：//详细可以看另一篇博客，axios的未解之谜
axios({
    url: '/getUsers',
    method: 'get',
    responseType: 'json', // 默认的
    data: {
       //'a': 1,
       //'b': 2,
      }
    }).then(function (response) {
        console.log(response);
        console.log(response.data);
    }).catch(function (error) {
        console.log(error);
}）
```
```
ajax： //详细可以看另一篇博客  ajax笔记
$.ajax({
            url: '/getUsers',
            type: 'get',
            dataType: 'json',
            data: {
                //'a': 1,
                //'b': 2,
            },
            success: function (response) {
                console.log(response)；
            }
        })
```
## 优缺点：

ajax：

本身是针对**MVC**的编程,不符合现在前端MVVM的浪潮

**基于原生的XHR开发（ajax笔记篇中有详细的内容）**，XHR本身的架构不清晰，已经有了fetch的替代方案

JQuery整个项目太大，单纯使用ajax却要引入整个JQuery非常的不合理（采取个性化打包的方案又不能享受CDN服务

axios：

从 node.js 创建 http 请求

支持 Promise API

客户端支持防止CSRF

提供了一些并发请求的接口（重要，方便了很多的操作）

