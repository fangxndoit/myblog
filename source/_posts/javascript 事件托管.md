---
title: javascript 事件托管 
tags: 事件委托,时间托管
grammar_cjkRuby: true
---

#### 事件委托
事件委托是指利用**事件冒泡**，指定一个事件处理程序，管理某一类型的事件
#### 事件冒泡
在 document.addEventListener 的时候我们可以设置事件模型：**事件冒泡**、**事件捕获**

![事件模型](http://jbcdn2.b0.upaiyun.com/2017/05/a6f74035ad4fd5dae82fdb4ed2761287.png)

#### 事件模型
* 捕获阶段：在事件冒泡的模型中，捕获阶段不会相应任何事件；
* 目标阶段：事件相应到触发事件的最底层元素上
* 冒泡阶段：事件的触发相应会从最底层目标一层层地向外到最外层（根节点），事件代理就是利用事件冒泡的机制把里层所需要相应的事件绑定到外层

#### 常见例子
```javascript
document.getElementById('menu').onclick = function(e) {
    //兼容IE
    e = e || window.event;
    var target = e.target || e.srcElement;

    if (target.nodeName !== 'A') {
        return;
    }

    ... // 需要对a标签所做的处理
}
```
```javascript
document.onclick = function(e) {
    //兼容IE
    e = e || window.event;
    var target = e.target || e.srcElement,
        targetNodeName = target.nodeName;

    if (targetNodeName == 'A') {
        // 一些处理...
    }elseif(targetNodeName == 'li'){
        // 一些处理...
    }else{
        // 一些处理...
    }
}
```