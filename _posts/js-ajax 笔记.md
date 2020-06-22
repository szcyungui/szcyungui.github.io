
---
layout:     post
title:  JS_AJAX笔记
date:       2020-06-24
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: 
    - Vue
    - React
    - 函数式编程
    - ES6
---
# Ajax简介

感觉这部分的知识呢~，已经看过一遍了，但是苦于一直没有应用，所以又忘了，所以接下来就准备再过一遍知识点，思考完后，进行一些代码的练习，把网站上的样例代码全部编写一遍应该就差不多了。

**首先：AJAX 是开发者的梦想，因为您能够：**

* **不刷新页面更新网页**
* **在页面加载后从服务器请求数据**
* **在页面加载后从服务器接收数据**
* **在后台向服务器发送数据**

先看一个样例：

```
<!DOCTYPE html>
<html>
<body>

<div id="demo">
<h1>XMLHttpRequest 对象</h1>
<button type="button" onclick="loadDoc()">修改内容</button>
</div>

<script>
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("demo").innerHTML =
      this.responseText;
    }
  };
  xhttp.open("GET", "/example/js/ajax_info.txt", true);
  xhttp.send();
}
</script>
</body>
</html>
```
这张 HTML 页面包含一个 <div> 和一个 <button>。
<div> 用于显示来自服务器的信息。

<button> 调用函数（当它被点击）。

该函数从 web 服务器请求数据并显示它：

```
Function loadDoc()
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
     document.getElementById("demo").innerHTML = this.responseText;
    }
  };
  xhttp.open("GET", "ajax_info.txt", true);
  xhttp.send();
} 
```
## 什么是Ajax?

AJAX = Asynchronous JavaScript And XML.

AJAX 并非编程语言。

AJAX 仅仅组合了：

* 浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）
* JavaScript 和 HTML DOM（显示或使用数据）

Ajax 是一个令人误导的名称。Ajax 应用程序可能使用 XML 来传输数据，但将数据作为纯文本或 JSON 文本传输也同样常见。Ajax 允许通过与场景后面的 Web 服务器交换数据来异步更新网页。这意味着可以更新网页的部分，而不需要重新加载整个页面。

**简而言之：**Ajax实际上是一种和后端或者说服务器交互的方式，这种方式和传统的click-fresh不同，它允许web页面进行异步的请求和参数的更新。

## **AJAX 如何工作**

1. 网页中发生一个事件（页面加载、按钮点击）
2. 由 JavaScript 创建 XMLHttpRequest 对象
3. XMLHttpRequest 对象向 web 服务器发送请求
4. 服务器处理该请求
5. 服务器将响应发送回网页
6. 由 JavaScript 读取响应
7. 由 JavaScript 执行正确的动作（比如更新页面）
# AJAX - XMLHttpRequest 对象

**Ajax 的核心是 XMLHttpRequest 对象。**

## XMLHttpRequest 对象

所有现代浏览器都支持 XMLHttpRequest 对象。

XMLHttpRequest 对象用于同幕后服务器交换数据。这意味着可以更新网页的部分，而不需要重新加载整个页面。

## 创建 XMLHttpRequest 对象

所有现代浏览器（Chrom、IE7+、Firefox、Safari 以及 Opera）都有内建的 XMLHttpRequest 对象。

创建 XMLHttpRequest 的语法是：

```
variable = new XMLHttpRequest();
```
老版本的 Internet Explorer（IE5 和 IE6）使用 ActiveX 对象：
```
variable = new ActiveXObject("Microsoft.XMLHTTP");
```
为了应对所有浏览器，包括 IE5 和 IE6，请检查浏览器是否支持 XMLHttpRequest 对象。如果支持，创建 XMLHttpRequest 对象，如果不支持，则创建 ActiveX 对象：
**实例**

```
var xhttp;
if (window.XMLHttpRequest) {
    xhttp = new XMLHttpRequest();
    } else {
    // code for IE6, IE5
     xhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```
## 跨域访问

出于安全原因，现代浏览器不允许跨域访问。

这意味着尝试加载的网页和 XML 文件都必须位于相同服务器上。

W3School 上的实例都会打开位于 W3School 域上的 XML 文件。

如果希望在自己的页面使用以上实例，那么所加载的 XML 文件必须位于您自己的服务器上。

## XMLHttpRequest 对象方法

| 方法   | 描述   | 
|:----|:----|
| new XMLHttpRequest()   | 创建新的 XMLHttpRequest 对象   | 
| abort()   | 取消当前请求   | 
| getAllResponseHeaders()   | 返回头部信息   | 
| getResponseHeader()   | 返回特定的头部信息   | 
| open(*method*, *url*, *async*, *user*, *psw*)   | 规定请求method：请求类型 GET 或 POSTurl：文件位置async：true（异步）或 false（同步）user：可选的用户名称psw：可选的密码 | 
| send()   | 将请求发送到服务器，用于 GET 请求   | 
| send(*string*)   | 将请求发送到服务器，用于 POST 请求   | 
| setRequestHeader()   | 向要发送的报头添加标签/值对   | 

## XMLHttpRequest 对象属性

| 属性   | 描述   | 
|:----|:----|
| onreadystatechange   | 定义当 readyState 属性发生变化时被调用的函数   | 
| readyState   | 保存 XMLHttpRequest 的状态。0：请求未初始化1：服务器连接已建立2：请求已收到3：正在处理请求4：请求已完成且响应已就绪 | 
| responseText   | 以字符串返回响应数据   | 
| responseXML   | 以 XML 数据返回响应数据   | 
| status   | 返回请求的状态号200: "OK"403: "Forbidden"404: "Not Found"如需完整列表请访问 [Http 消息参考手册](https://www.w3school.com.cn/tags/ref_httpmessages.asp)   | 
| statusText   | 返回状态文本（比如 "OK" 或 "Not Found"）   | 

# AJAX - 向服务器发送请求

**XMLHttpRequest 对象用于同服务器交换数据。**

## 向服务器发送请求

如需向服务器发送请求，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

```
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```
| 方法   | 描述   | 
|:----|:----:|
| open(*method*, *url*, *async*)   | 规定请求的类型*method*：请求的类型：GET 还是 POST*url*：服务器（文件）位置*async*：true（异步）或 false（同步） | 
| send()   | 向服务器发送请求（用于 GET）   | 
| send(*string*)   | 向服务器发送请求（用于 POST）   | 

## GET 还是 POST？

GET 比 POST 更简单更快，可用于大多数情况下。

不过，请在以下情况始终使用 POST：

* 缓存文件不是选项（更新服务器上的文件或数据库）
* 向服务器发送大量数据（POST 无大小限制）
* 发送用户输入（可包含未知字符），POST 比 GET 更强大更安全
## GET 请求

一条简单的 GET 请求：

**实例**

```
xhttp.open("GET", "demo_get.asp", true);
xhttp.send();
```
在上面的例子中，您可能会获得一个缓存的结果。为了避免此情况，请向 URL 添加一个唯一的 ID：
**实例**

```
xhttp.open("GET", "demo_get.asp?t=" + Math.random(), true);
xhttp.send();
```
如果您需要用 GET 方法来发送信息，请向 URL 添加这些信息：
**实例**

```
xhttp.open("GET", "demo_get2.asp?fname=Bill&lname=Gates", true);
xhttp.send();
```
## POST 请求

一条简单的 POST 请求：

**实例**

```
xhttp.open("POST", "demo_post.asp", true);
xhttp.send();
```
如需像 HTML 表单那样 POST 数据，请通过 setRequestHeader() 添加一个 HTTP 头部。请在 send() 方法中规定您需要发送的数据：
**实例**

```
xhttp.open("POST", "ajax_test.asp", true);
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhttp.send("fname=Bill&lname=Gates");
```
| 方法   | 描述   | 
|:----|:----:|
| setRequestHeader(*header*, *value*)   | 向请求添加 HTTP 头部*header*：规定头部名称*value*：规定头部值 | 

## *url* - 服务器上的文件

open() 方法的 *url* 参数，是服务器上文件的地址：

```
xhttp.open("GET", "ajax_test.asp", true);
```
该文件可以是任何类型的文件，如 .txt 和 .xml，或服务器脚本文件，如 .asp 和 .php（它们可以在发送回响应之前在服务器执行操作）。
## 异步 - ture 还是 false？

如需异步发送请求，open() 方法的 *async* 参数必须设置为 true：

```
xhttp.open("GET", "ajax_test.asp", true);
```
发送异步请求对 web 开发人员来说是一个巨大的进步。服务器上执行的许多任务都非常耗时。在 AJAX 之前，此操作可能会导致应用程序挂起或停止。
通过异步发送，JavaScript 不必等待服务器响应，而是可以：

* 在等待服务器响应时执行其他脚本
* 当响应就绪时处理响应
## onreadystatechange 属性

通过 XMLHttpRequest 对象，您可以定义当请求接收到应答时所执行的函数。

这个函数是在 XMLHttpResponse 对象的 onreadystatechange 属性中定义的：

**实例**

```
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.responseText;
  }
};
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send();
```
## 同步请求

如需执行同步的请求，请把 open() 方法中的第三个参数设置为 false：

```
xhttp.open("GET", "ajax_info.txt", false);
```
有时 async = false 用于快速测试。你也会在更老的 JavaScript 代码中看到同步请求。
由于代码将等待服务器完成，所以不需要 onreadystatechange 函数：

**实例**

```
xhttp.open("GET", "ajax_info.txt", false);
xhttp.send();
document.getElementById("demo").innerHTML = xhttp.responseText;
```
# AJAX - 服务器响应

## onreadystatechange 属性

readyState 属性存留 XMLHttpRequest 的状态。

onreadystatechange 属性定义当 readyState 发生变化时执行的函数。

status 属性和 statusText 属性存有 XMLHttpRequest 对象的状态。

| 属性   | 描述   | 
|:----|:----|
| onreadystatechange   | 定义了当 readyState 属性发生改变时所调用的函数。   | 
| readyState   | 保存了 XMLHttpRequest 的状态。0: 请求未初始化1: 服务器连接已建立2: 请求已接收3: 正在处理请求4: 请求已完成且响应已就绪 | 
| status   | 200: "OK"403: "Forbidden"404: "Page not found"如需完整列表，请访问 [Http 消息参考手册](https://www.w3school.com.cn/tags/ref_httpmessages.asp)   | 
| statusText   | 返回状态文本（例如 "OK" 或 "Not Found"）   | 

每当 readyState 发生变化时就会调用 onreadystatechange 函数。

当 readyState 为 4，status 为 200 时，响应就绪：

**实例**

```
function loadDoc() {
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            document.getElementById("demo").innerHTML =
            this.responseText;
       }
    };
    xhttp.open("GET", "ajax_info.txt", true);
    xhttp.send(); 
} 
```
## 使用回调函数

回调函数是一种作为参数被传递到另一个函数的函数。

如果您的网站中有多个 AJAX 任务，那么您应该创建一个执行 XMLHttpRequest 对象的函数，以及一个供每个 AJAX 任务的回调函数。

该函数应当包含 URL 以及当响应就绪时调用的函数。

**实例**

```
loadDoc("url-1", myFunction1);
loadDoc("url-2", myFunction2);
function loadDoc(url, cFunction) {
  var xhttp;
  xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      cFunction(this);
    }
  };
  xhttp.open("GET", url, true);
  xhttp.send();
}
function myFunction1(xhttp) {
  // action goes here
 } 
function myFunction2(xhttp) {
  // action goes here
 } 
```
Tips:所谓的回调函数只听名字未免有些高大上，但是实际上很好理解，比如你要计算a*b，你首先需要从服务器端拿到两个变量的值对吧，这个过程假如是post1，计算过程假如是cal1，那cal1如果在post1没有结束的时候就运行，那么浏览器肯定会报错“Not a certain value！”所以整个过程必须保证cal1在post1结束后再进行，那么也就是说if(post1 sucess!){cal1}，这也就是所谓的回调过程。
## 服务器响应属性

| 属性   | 描述   | 
|:----|:----|
| responseText   | 获取字符串形式的响应数据   | 
| responseXML   | 获取 XML 数据形式的响应数据   | 

## 服务器响应方法

| 方法   | 描述   | 
|:----|:----|
| getResponseHeader()   | 从服务器返回特定的头部信息   | 
| getAllResponseHeaders()   | 从服务器返回所有头部信息   | 

## responseText 属性

responseText 属性以 JavaScript 字符串的形式返回服务器响应，因此您可以这样使用它：

**实例**

```
document.getElementById("demo").innerHTML = xhttp.responseText;
```
## responseXML 属性

**XML HttpRequest** 对象有一个內建的 XML 解析器。

**ResponseXML **属性以 XML DOM 对象返回服务器响应。

使用此属性，您可以把响应解析为 XML DOM 对象：

**实例**

请求文件 [music_list.xml](https://www.w3school.com.cn/demo/music_list.xml)，并对响应进行解析：

```
xmlDoc = xhttp.responseXML;
txt = "";
x = xmlDoc.getElementsByTagName("ARTIST");
for (i = 0; i < x.length; i++) {
  txt += x[i].childNodes[0].nodeValue + "<br>";
  }
document.getElementById("demo").innerHTML = txt;
xhttp.open("GET",  "music_list.xml", true);
xhttp.send();
```
## getAllResponseHeaders() 方法

**getAllResponseHeaders() **方法返回所有来自服务器响应的头部信息。

**实例**

```
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.getAllResponseHeaders();
  }
};
```
## getResponseHeader() 方法

**getResponseHeader() **方法返回来自服务器响应的特定头部信息。

**实例**

```
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("demo").innerHTML = this.getResponseHeader("Last-Modified");
  }
};
xhttp.open("GET", "ajax_info.txt", true);
xhttp.send(); 
```
# AJAX XML 实例

**AJAX 可用于同 XML 文件进行交互式通信。**

* 当用户点击“获取 CD 信息”按钮时，执行 loadDoc() 函数。
* loadDoc() 函数创建 XMLHttpRequest 对象，添加当服务器响应就绪时执行的函数，并向服务器发送请求。
* 当服务器响应就绪后，构建 HTML 表格，从 XML 文件提取节点（因素），最后使用由 XML 数据填充的 HTML 表格对元素“demo”进行更新：
## LoadXMLDoc()

```
function loadDoc() {
  var xhttp = new XMLHttpRequest();
   xhttp.onreadystatechange = function() {
    if (this.readyState  == 4 && this.status == 200) {
    myFunction(this);
     }
  };
  xhttp.open("GET", "music_list.xml", true);
  xhttp.send();
}
function myFunction(xml) {
  var i;
  var xmlDoc = xml.responseXML;
  var table="<tr><th>艺术家</th><th>曲目</th></tr>";
  var x = xmlDoc.getElementsByTagName("TRACK");
  for (i = 0; i <x.length;  i++) { 
    table += "<tr><td>" +
    x[i].getElementsByTagName("ARTIST")[0].childNodes[0].nodeValue  +
    "</td><td>" +
    x[i].getElementsByTagName("TITLE")[0].childNodes[0].nodeValue  +
    "</td></tr>";
  }
   document.getElementById("demo").innerHTML = table;
} 
```
## XML 文件

上例中使用的 XML 文件类似这样："[music_list.xml](https://www.w3school.com.cn/demo/music_list.xml)"。

# AJAX-PHP交互实例

下面的例子演示：当用户在输入字段中键入字符时，网页如何与 web 服务器进行通信：

```
<html>
<head>
<script>
function showHint(str) {
  var xhttp;
  if (str.length == 0) { 
    document.getElementById("txtHint").innerHTML = "";
    return;
  }
  xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function(){
  if (this.readyState == 4 && this.status == 200) {
    document.getElementById("txtHint").innerHTML = this.responseText;
  }
};
  xhttp.open("GET", "/demo/gethint.php?q="+str, true);
  xhttp.send();

}
</script>
</head>
<body>
<h1>XMLHttpRequest 对象</h1>
<h2>请在下面的输入字段中键入字母 A-Z：</h2>
<p>搜索建议：<span id="txtHint"></span></p> 
<p>姓名：<input type="text" id="txt1" onkeyup="showHint(this.value)"></p>
</body>
</html>
```
### 代码解释：

首先，检查输入字段是否为空（str.length == 0），如果是，清空 txtHint 占位符的内容并退出函数。

不过，如果输入字段不为空，则进行如下：

* 创建 XMLHttpRequest 对象
* 创建当服务器响应就绪时执行的函数
* 发送请求到服务器上的 PHP 文件（gethint.php）
* 请注意添加到 gethint.php 的 q 参数
* str 变量保存了输入字段的内容




