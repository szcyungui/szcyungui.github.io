---
layout:     post
title:    Element_UI 源样式修改（持续更新）
date:       2020-06-23
author:     Yungui
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags: 
    - Vue
    - React
    - 函数式编程
    - ES6
---

## Input篇
通过F12或者其他方式进入浏览器调试界面，可以发现在界面中,el-input除了其本身，还存在着el-input__inner部分的CSS样式，只有修改这部分样式才能使得原本的UI内部样式发生改变，所以我们进行了如下的样式修改：
``` javascript

.el-input__inner {
    height: 33px;
    font-size: 13px;
    box-shadow: none;
    border: 1px solid #e9e9e9;
}
 
.el-input__inner:hover {
    border-color: #e9e9e9;
}
 
.el-input__inner:focus {
    border-color: #42a5f5;
    box-shadow: none;
    transition-duration: .5s;
}
 
.el-input__inner::-webkit-input-placeholder {
    line-height: 20px;
}
 
.el-input__inner, .el-checkbox__inner, .el-textarea__inner, .el-button {
    border-radius: 0;
}
```

## Button篇
button修改高度后，文本无法竖直居中在文本内部
``` javascript
/*修改button的宽度可以直接采用width：xxxpx，但是很多时候会发现一个问题，height修改完成后，文字往往无法竖直居中在button的内部*/
.el-button{
        line-height: 0.5; // line-height属性在button中默认是1 ，测试后发现直接调整这个就可以实现按钮高度的调整，并且文本居中
    }

```
