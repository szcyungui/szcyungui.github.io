---
layout:     post
title:    MVVM,MVC,MVP......
date:       2020-06-26
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: 
    - Vue
    - React
    - 函数式编程
    - ES6
---

转载并修改：

[https://www.jianshu.com/p/b0aab1ffad93](https://www.jianshu.com/p/b0aab1ffad93),

[https://www.jianshu.com/p/220729f01a25](https://www.jianshu.com/p/220729f01a25)

### 前言

mvc和mvvm大概是个老生常谈的问题了，关于MVC和MVVM如此这般设计的原因以及我们应该如何思考一些相关的问题

### 1.前端的发展历史

在上个世纪的1989年，欧洲核子研究中心的物理学家Tim Berners-Lee发明了超文本标记语言（HyperText Markup Language），简称HTML，并在1993年成为互联网草案。从此，互联网开始迅速商业化，诞生了一大批商业网站。

 

最早的HTML页面是完全静态的网页，它们是预先编写好的存放在Web服务器上的html文件。浏览器请求某个URL时，Web服务器把对应的html文件扔给浏览器，就可以显示html文件的内容了。

 

如果要针对不同的用户显示不同的页面，显然不可能给成千上万的用户准备好成千上万的不同的html文件，所以，服务器就需要针对不同的用户，动态生成不同的html文件。一个最直接的想法就是利用C、C++这些编程语言，直接向浏览器输出拼接后的字符串。这种技术被称为CGI：Common Gateway Interface。

 

很显然，像新浪首页这样的复杂的HTML是不可能通过拼字符串得到的。于是，人们又发现，其实拼字符串的时候，大多数字符串都是HTML片段，是不变的，变化的只有少数和用户相关的数据，所以，又出现了新的创建动态HTML的方式：ASP、JSP和PHP——分别由微软、SUN和开源社区开发。

 

在ASP中，一个asp文件就是一个HTML，但是，需要替换的变量用特殊的<%=var%>标记出来了，再配合循环、条件判断，创建动态HTML就比CGI要容易得多。

 

但是，一旦浏览器显示了一个HTML页面，要更新页面内容，唯一的方法就是重新向服务器获取一份新的HTML内容。如果浏览器想要自己修改HTML页面的内容，就需要等到1995年年底，JavaScript被引入到浏览器。

 

有了JavaScript后，浏览器就可以运行JavaScript，然后，对页面进行一些修改。JavaScript还可以通过修改HTML的DOM结构和CSS来实现一些动画效果，而这些功能没法通过服务器完成，必须在浏览器实现。

 

用JavaScript在浏览器中操作HTML，经历了若干发展阶段：

 第一阶段，直接用JavaScript操作DOM节点，使用浏览器提供的原生API：

 第二阶段，由于原生API不好用，还要考虑浏览器兼容性，jQuery横空出世，以简洁的API迅速俘获了前端开发者的芳心：

 第三阶段，MVC模式，需要服务器端配合，JavaScript可以在前端修改服务器渲染后的数据。

 现在，随着前端页面越来越复杂，用户对于交互性要求也越来越高，想要写出Gmail这样的页面，仅仅用jQuery是远远不够的。MVVM模型应运而生。

 

MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离。

### 2.从MVC开始

几乎所有的App都只干这么一件事：**将数据展示给用户看，并处理用户对界面的操作。**

 **MVC的思想**：一句话描述就是**Controller负责将Model的数据用View显示出来**，换句话说就是在Controller里面把Model的数据赋值给View，比如在controller中写document.getElementById("box").innerHTML = data[”title”]，只是还没有刻意建一个Model类出来而已。

![图片](https://uploader.shimo.im/f/87Pn02lAHa1YcG8B.png!thumbnail)


---
**M、V、C**

**Model（模型）**：是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据。

**View（视图）**：是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的。

**Controller（控制器）**：是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。

![图片](https://uploader.shimo.im/f/X0Uf39Niz01s6spg.png!thumbnail)

[https://upload-images.jianshu.io/upload_images/7770244-f4fc955f1488299a.png?imageMogr2/auto-orient/strip](https://upload-images.jianshu.io/upload_images/7770244-f4fc955f1488299a.png?imageMogr2/auto-orient/strip)|imageView2/2/format/webp

这张图把MVC分为三个独立的区域，并且中间用了一些线来隔开。很有意思的设计，因为这些线似乎出现在了驾校科目一的内容中，你瞧C和V以及C和M之间的白线，一部分是虚线一部分是实线对吧，这就表明了引用关系：C可以直接引用V和M，而V和M不能直接引用C，至少你不能显式的在V和M的代码中去写和C相关的任何代码，而V和M之间则是双黄线，没错，它们俩谁也不能引用谁，你既不能在M里面写V，也不能在V里面写M。哦，上面的描述有点小小的问题，你不是“不能”这样写，而是“不应该”这样写，没人能阻止你在写代码的时候在一个M里面去写V，但是一旦你这样做了，那么你就违背了MVC的规范，你就不是在使用MVC了，所以这算是MVC的一个必要条件：使用MVC –> M里面没有V的代码。所以M里面没有V的代码就是使用MVC的必要条件。

**View和Controller的交互**

 

按钮点击事件，是View来接收的，但是处理这个事件的应该是Controller，所以View把这个事件传递给了Controller，如何传递的呢，见图，看到View上面的action没有，这就是事件，看到Controller上面的target没有，这就是靶子，View究竟要把事件传递给谁，它被规定了传递给靶子，Controller实际上就是靶子。只是View只负责传递事件，不负责关心靶子是谁。就像你是一个负责运货的少年，你唯一知道的是你要把货（action）交给上头（开发者）告诉你的那个收货的人（target），至于那个收货的人是警察还是怪兽，你都不需要关心。这是V和C的一种交互方式，叫做target-action。所以你看，这张图简直就是神来之笔，旁边还栩栩如生的画出了V对C的另一种传值：协议-委托。委托有两种：代理和数据源。什么是代理，就是专门处理should、will、did事件的委托，什么是数据源，就是专门处理data、count等等的委托。

  

**Model和Controller的交互**

 

M是干嘛的？上面说了，M就是数据管理者，你可以理解为它直接和数据库打交道。这里的数据库可能是本地的，也可能是服务器上的，M会从数据库获取数据，也可能把数据上传给数据库。M也将提供属性或者接口来供C访问其持有的数据。我们就拿一个简单的需求作为例子，假如我想在一个模块中显示一段文字，这段文字是从网上获取下来的。

 

那么使用MVC的话，在C中肯定需要一个UILabel（V）作为属性来显示这段文字，而这段文字由谁来获取呢，肯定是由M来获取了。而获取的地方在哪里呢？通常在C的生命周期里面，所以往往是在C的一个生命周期方法比如viewDidLoad里面调用M获取数据的方法来获取数据。现在问题来了，M获取数据的方法是异步的网络请求，网络请求结束后，C才应该用请求下来的数据重新赋值给V，现在的问题是，C如何知道网络请求结束了？

 

这里我们一定要换一种角度去思考，我们进一步考虑M和V之间的关系：它们应该是一种同步的关系，也就是，不管任何时刻，只要M的值发生改变，V的显示就应该发生改变（显示最新的M的内容）。所以我们可以关注M的值改变，而不用关心M的网络请求是否结束了。实际上C根本不知道M从哪去拿的数据，C的责任是负责把M最新的数据赋值给V。所以C应该关注的事件是：M的值是否发生了变化。

1. MVP

[https://www.jianshu.com/p/b4b6e2dbac2a](https://www.jianshu.com/p/b4b6e2dbac2a)  参考这个吧

怎么说呢？对MVP几乎从来没有使用过相关的框架，所以没有很深刻的理解，我根据查询的资料来分析，可能也是将view层原本直接从model获取数据的部分功能进行了删减，现在无法实现M->V的互通了，这部分工作就交给了原本的Controller逐渐演变成了现在的Presenter ，解释到这里，我突然发现，貌似我之前接触过的一个 springboot 项目采用的就是上面的这种模式，但是时间太久就忘记了，等我回顾一下在做补充。

![图片](https://uploader.shimo.im/f/xWIZz3nsF23HLmdE.png!thumbnail)

1. MVMM

真正的重点总要放到最后再来讲述：

MVVM 模式将 Presenter 改名为 ViewModel，基本上与 MVP 模式完全一致。

![图片](https://uploader.shimo.im/f/njLdhj5dHju5KM8B.png!thumbnail)

![图片](https://uploader.shimo.im/f/nwLHSXuiEvo0hKV1.png!thumbnail)

唯一的区别是，它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然。

这里面使用的设计模式有：观察者模式（发布订阅模式）、代理模式、工厂模式、单例模式。

**双向绑定**

在 MVVM 中，UI 是通过数据驱动的，数据一旦改变就会相应的刷新对应的 UI，UI 如果改变，也会改变对应的数据。这种方式就可以在业务处理中只关心数据的流转，而无需直接和页面打交道。ViewModel 只关心数据和业务的处理，不关心 View 如何处理数据，在这种情况下，View 和 Model 都可以独立出来，任何一方改变了也不一定需要改变另一方，并且可以将一些可复用的逻辑放在一个 ViewModel 中，让多个 View 复用这个 ViewModel。

说到这个双向绑定，让我一下子就想起来了Vue中的v-model属性，这不就是解释这个的最好的例子吗？当input的数值发生变化，不需要什么onchange函数这些，只要你修改了input内的值，data中的数值就会直接随之变化。





