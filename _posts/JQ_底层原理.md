---
layout:     post
title:   JQ_底层原理
date:       2020-07-27
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: 
    - JQ
    - 函数式编程
    - ES6
---
# jQuery的基本结构 

```plain
/* 
1.jQuery的本质是一个闭包 
2.jQuery为什么要使用闭包来实现? 
  为了避免多个框架的冲突 
3.jQuery如何让外界访问内部定义的局部变量 
   window.xxx = xxx; 
4.jQuery为什么要给自己传递一个window参数? 
   为了方便后期压缩代码 
   为了提升查找的效率 
5.jQuery为什么要给自己接收一个undefined参数? 
   为了方便后期压缩代码 
   IE9以下的浏览器undefined可以被修改, 为了保证内部使用的undefined不被修改, 所以需要接收一个正确的undefined 
*/ 
(function( window, undefined ) { 
    var jQuery = function( ) { 
        return new jQuery.prototype.init( ); 
    } 
    jQuery.prototype = { 
        constructor: jQuery 
    } 
    jQuery.prototype.init.prototype = jQuery.prototype; 
    window.jQuery = window.$ = jQuery; 
})( window ); 
```
jQuery入口函数测试 

```plain
/* 
 1.传入 '' null undefined NaN  0  false, 返回空的jQuery对象 
 2.字符串: 
   代码片段:会将创建好的DOM元素存储到jQuery对象中返回 
   选择器: 会将找到的所有元素存储到jQuery对象中返回 
 3.数组: 
   会将数组中存储的元素依次存储到jQuery对象中立返回 
 4.除上述类型以外的: 
   会将传入的数据存储到jQuery对象中返回 
*/ 
```









# Tips: 

```plain
// trim()内置方法在IE9以下不支持 所以下面是手写的替代兼容方法 
trim : function(str){ 
    if(!njQuery.isString(str)){ 
        return str; 
    } 
    // 判断是否支持trim方法 
    if(str.trim){ 
        return str.trim(); 
    }else{ 
        return str.replace(/^\s+|\s+$/g, ""); 
    } 
} 
```
```javascript
if(njQuery.isHTML(selector)){ 
    // 1.根据代码片段创建所有的元素 
    var temp = document.createElement("div"); 
    temp.innerHTML = selector; 
    /* 
    // 2.将创建好的一级元素添加到jQuery当中 
    for(var i = 0; i < temp.children.length; i++){ 
        this[i] = temp.children[i]; 
    } 
    // 3.给jQuery对象添加length属性 
    this.length = temp.children.length; 
    */ 
    [].push.apply(this, temp.children); 
    // 此时此刻的this是njQuery对象 
    // 4.返回加工好的this(jQuery) 
    // return this; 
} 
```
```javascript
// 真数组和伪数组的相互转化 
// 真数组转换伪数组的一个过程 
/* 
1.通过[].push找到数组中的push方法 
2.通过apply(obj)将找到的push方法内部的this修改为自定义的对象 
3.将传入数组中的元素依次取出, 传递给形参 
*/ 
var arr = [1, 3, 5, 7, 9]; 
var obj = {}; 
[].push.apply(obj, arr); 
console.log(obj); 
// 如果想将伪数组转换为真数组那么可以使用如下方法 
var obj = {0:"lnj", 1:"33", length: 2}; 
var arr = [].slice.call(obj); 
console.log(arr); 
```
