---
title: vue生命周期 
tags: vue,生命周期
grammar_cjkRuby: true
---


![vue生命周期](https://cn.vuejs.org/images/lifecycle.png)

#### 翻译
|状态  | 结果 |
|------------- | -------------|
| new vue | 创建vue实例 |
| init events & lifecycle | 开始初始化 |
| beforeCreate | 组件刚被创建，组建属性计算之前，如data属性等 |
| init injections & reactivity | 通过依赖注入导入依赖项 |
| created | 组件实例创建完成，属性已绑定，此时DOM还未生成 |
| el属性 | 检查vue配置，即new Vue{}里面的el项是否存在，有就继续检查template项。没有则等到手动绑定调用vm.$mount() |
| template | 检查配置中的template项，如果没有template进行填充被绑定区域，则被绑定区域的el对象的outerHTML（即整个#app DOM对象，包括<div id=”app” >和</div>标签）都作为被填充对象替换掉填充区域 |
| beforeMount | 模板编译、挂载之前 |
| create vm.$el and replace “el” with it | 编译，并替换了被绑定元素 |
| mounted | 编译、挂载 |
| Before update | 组件更新之前 |
| updated | 组件更新之后 |
| destroy | 当vm.$destroy()被调用，开始拆卸组件和监听器，生命周期终结 |


[表格标题]

#### 生命周期的历程
* new Vue： 创建vue实例
* beforecreated： 监测数据，初始化事件
* created：绑定数据和事件，此时可以访问修改data，访问methods
* beforeMount： 模板编译，生成 dom 但未挂载
* mounted： 编译，挂载，实例数据在dom上渲染
* beforeUpdate： 数据更新，但不进行DOM重新渲染
* updated： 更新数据，进行dom渲染
* beforeDestory： 实例销毁
* destroyed： 实例销毁，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁




#### 什么时候需要用到Vue.nextTick()

1. 在 Vue 生命周期的 created() 钩子函数进行的 DOM 操作一定要放在 Vue.nextTick() 的回调函数中。原因是什么呢，原因是
在 created() 钩子函数执行的时候 DOM 其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，所以此处一定要将 DOM 操作
的 js 代码放进 Vue.nextTick() 的回调函数中。与之对应的就是 mounted 钩子函数，因为该钩子函数执行时所有的 DOM 挂载和
渲染都已完成，此时在该钩子函数中进行任何DOM操作都不会有问题 。
2. 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的 DOM 结构的时候，这个操作都应该放
进 Vue.nextTick() 的回调函数中。