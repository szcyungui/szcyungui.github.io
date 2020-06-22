---
layout:     post
title:    VueX-Vue状态管理
date:       2020-06-22
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: Vue,React,函数式编程,ES6
    - Vue
    - React
    - 函数式编程
    - ES6
---

## 1.  Vue的状态管理模式
### 1.1  关于VueX 

转载链接：[https://www.jianshu.com/p/2e5973fe1223](https://www.jianshu.com/p/2e5973fe1223)  另有补充

VueX是适用于在Vue项目开发时使用的状态管理工具。试想一下，如果在一个项目开发中频繁的使用组件传参的方式来同步data中的值，一旦项目变得很庞大，管理和维护这些值将是相当棘手的工作。为此，Vue为这些被多个组件频繁使用的值提供了一个统一管理的工具——VueX。在具有VueX的Vue项目中，我们只需要把这些值定义在VueX中，即可在整个Vue项目的组件中使用。

与react中的redux是相同的含义，我们都知道无论是react还是vue都追求组件化开发，高内聚，低耦合的设计思路让我们在开发过程中获得了很多的便利，但是组件之间的关联是不可避免的，因为我们需要完成的是一个整体，主要的关联是通过state或者说页面的绑定数据实现的，其中最常用的无非是几种传递模式：父子组件之间的参数传递，同级组件之间的参数传递，深层组件与表层组件之间的参数传递，尽管我们都可以通过各种方法，例如props等实现，但是复杂度和混乱程度让我们的项目难以进行后期维护和管理。

那么我们有没有可能把所有的state都放在一个文件下，或者一个文件夹下，谁需要什么数据就从这个store中进行调用呢？这就是redux和vueX的设计理念了。

### 1.2 安装

以下步骤的前提是你已经完成了Vue项目构建，并且已转至该项目的文件目录下。

Npm安装Vuex（百度即可，安装vue2.0 或者 3.0 安装方式并不相同）

在项目的根目录下新增一个store文件夹，在该文件夹内创建index.js

此时你的项目的src文件夹应当是这样的

```
│  App.vue
│  main.js
│
├─assets
│      logo.png
│
├─components
│      HelloWorld.vue
│
├─router
│      index.js
│
└─store
       index.js
```
### 1.3 使用

1.3.1 初始化store下index.js中的内容

```
import Vue from 'vue'
import Vuex from 'vuex'
//挂载Vuex
Vue.use(Vuex)
//创建VueX对象
const store = new Vuex.Store({
    state:{
        //存放的键值对就是所要管理的状态
        name:'helloVueX'
    }
})
export default store
```
1.3.2 将store挂载到当前项目的Vue实例当中去
打开main.js

```
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store'
Vue.config.productionTip = false
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  store,  //store:store 和router一样，将我们创建的Vuex实例挂载到这个vue实例中
  render: h => h(App)
})
```
 
1.3.3 在组件中使用Vuex

 

例如在App.vue中，我们要将state中定义的name拿来在h1标签中显示

 

```
<template>
    <div id='app'>
        name:
        <h1>{{ $store.state.name }}</h1>
    </div>
</template>
```
 
或者要在组件方法中使用

 

```
...,
methods:{
    add(){
      console.log(this.$store.state.name)
    }
},
...
```
 
**注意，请不要在此处更改state中的状态的值，后文中将会说明**

### 1.4 安装Vue开发工具VueDevtools

在学习VueX时，更为需要使用该插件。关于该插件的使用可以移步官网，在此不再赘叙。

## 2.  VueX中的核心内容

在VueX对象中，其实不止有state,还有用来操作state中数据的方法集，以及当我们需要对state中的数据需要加工的方法集等等成员。

**成员列表：**

* state     存放状态
* mutations   state成员操作
* getters     加工state成员给外界
* actions     异步操作
* modules   模块化状态管理

其实也很好理解，上面的例子中我们只是单纯地使用了state数据，但是说到管理，我们肯定不能只局限于使用，在使用的过程中我们难免要涉及到对state状态的更新操作：删除，新增，更新等等。

### 2.1 VueX的工作流程

![图片](https://uploader.shimo.im/f/n5kmvBH54N5qMFtt.png!thumbnail)

（[https://upload-images.jianshu.io/upload_images/16550832-20d0ad3c60a99111.png?imageMogr2/auto-orient/strip](https://upload-images.jianshu.io/upload_images/16550832-20d0ad3c60a99111.png?imageMogr2/auto-orient/strip)|imageView2/2/format/webp）

首先，Vue组件如果调用某个VueX的方法过程中需要向后端请求时或者说出现异步操作时，需要dispatch VueX中actions的方法，以保证数据的同步。可以说，action的存在就是为了让mutations中的方法能在异步操作中起作用。

如果没有异步操作，那么我们就可以直接在组件内提交状态中的Mutations中自己编写的方法来达成对state成员的操作。注意，1.3.3节中有提到，不建议在组件中直接对state中的成员进行操作，这是因为直接修改(例如：this.$store.state.name = 'hello')的话不能被VueDevtools所监控到。最后被修改后的state成员会被渲染到组件的原位置当中去。

### 2.2 Mutations

**mutations**是操作**state**数据的方法的集合，比如对该数据的修改、增加、删除等等。

**2.2.1 Mutations使用方法**

mutations方法都有默认的形参：(**[state]** **[,payload]**)

*  state是当前VueX对象中的state 
*  payload是该方法在被调用时传递参数使用的

例如，我们编写一个方法，当被执行时，能把下例中的name值修改为"jack",我们只需要这样做

index.js

```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.store({
    state:{
        name:'helloVueX'
    },
    mutations:{
        //es6语法，等同edit:funcion(){...}
        edit(state){
            state.name = 'jack'
        }
    }
})
export default store
```
而在组件中，我们需要这样去调用这个mutation——例如在App.vue的某个method中:
```
this.$store.commit('edit')
```
**2.2.2 Mutation传值**
在实际生产过程中，会遇到需要在提交某个mutation时需要携带一些参数给方法使用。

 

单个值提交时:

 

```
this.$store.commit('edit',15)
```
当需要多参提交时，推荐把他们放在一个对象中来提交:
```
this.$store.commit('edit',{age:15,sex:'男'})
```
接收挂载的参数：
```
edit(state,payload){
    state.name = 'jack'
    console.log(payload) // 15或{age:15,sex:'男'}
}
```
 
**另一种提交方式**

```
this.$store.commit({
    type:'edit',
    payload:{
        age:15,
        sex:'男'
    }
})
```
2.2.3 增删state中的成员 
为了配合Vue的响应式数据，我们在Mutations的方法中，应当使用Vue提供的方法来进行操作。如果使用delete或者xx.xx = xx的形式去删或增，则Vue不能对数据进行实时响应。

Vue.set 为某个对象设置成员的值，若不存在则新增 

例如对state对象中添加一个age成员

```
Vue.set(state,"age",15)
```
Vue.delete 删除成员 
将刚刚添加的age成员删除

```
Vue.delete(state,'age')
```
### 2.3 Getters 

可以对state中的成员加工后传递给外界 

Getters中的方法有两个默认参数

* state 当前VueX对象中的状态对象
* getters 当前getters对象，用于将getters下的其他getter拿来用

例如

```
getters:{
    nameInfo(state){
        return "姓名:"+state.name
    },
    fullInfo(state,getters){
        return getters.nameInfo+'年龄:'+state.age
    }  
}
```
组件中调用
```
this.$store.getters.fullInfo
```
### 2.4 Actions

由于直接在mutation方法中进行异步操作，将会引起数据失效。所以提供了Actions来专门进行异步操作，最终提交mutation方法。 

Actions中的方法有两个默认参数

*  context 上下文(相当于箭头函数中的this)对象 
*  payload 挂载参数

例如，我们在两秒中后执行2.2.2节中的edit方法

由于setTimeout是异步操作，所以需要使用actions

```
actions:{
    aEdit(context,payload){
        setTimeout(()=>{
            context.commit('edit',payload)
        },2000)
    }
}
```
在组件中调用：
```
this.$store.dispatch('aEdit',{age:15})
```
**改进:**
由于是异步操作，所以我们可以为我们的异步操作封装为一个Promise对象

```
    aEdit(context,payload){
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                context.commit('edit',payload)
                resolve()
            },2000)
        })
    }
```
### 2.5 Models

当项目庞大，状态非常多时，可以采用模块化管理模式。Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。

```
models:{
    a:{
        state:{},
        getters:{},
        ....
    }
}
```
组件内调用模块a的状态：
```
this.$store.state.a
```
而提交或者dispatch某个方法和以前一样,会自动执行所有模块内的对应type的方法：
```
this.$store.commit('editKey')
this.$store.dispatch('aEditKey')
```
**2.5.1 模块的细节 **
模块中mutations和getters中的方法接受的第一个参数是自身局部模块内部的state

```
models:{
    a:{
        state:{key:5},
        mutations:{
            editKey(state){
                state.key = 9
            }
        },
        ....
    }
} 
```
getters中方法的第三个参数是根节点状态
```
models:{
    a:{
        state:{key:5},
        getters:{
            getKeyCount(state,getter,rootState){
                return  rootState.key + state.key
            }
        },
        ....
    }
}
```
actions中方法获取局部模块状态是context.state,根节点状态是context.rootState
```
models:{
    a:{
        state:{key:5},
        actions:{
            aEidtKey(context){
                if(context.state.key === context.rootState.key){
                    context.commit('editKey')
                }
            }
        },
        ....
    }
}
```
## 3. 规范目录结构

如果把整个store都放在index.js中是不合理的，所以需要拆分。比较合适的目录格式如下：

 

```
store:.
│  actions.js
│  getters.js
│  index.js
│  mutations.js
│  mutations_type.js   ##该项为存放mutaions方法常量的文件，按需要可加入
│
└─modules
        Astore.js
```
 
对应的内容存放在对应的文件中，和以前一样，在index.js中存放并导出store。state中的数据尽量放在index.js中。而modules中的Astore局部模块状态如果多的话也可以进行细分。



