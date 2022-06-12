---
title: vue 面试题（转载）
date: 2021-03-04 14:55:46
tags:
---

####**1.说说你对`SPA`单页页面的理解，它的优缺点分别是什么？**

`SPA（single-page application）`仅在Web页面初始化时加载相应的`HTML`、`JavaScript`和`CSS`。一旦页面加载完成，`SPA`不会因为用户操作而进行页面的重新加载和跳转；取而代之的是利用路由机制实现`HTML`内容的转换，`UI`与用户的交互，避免页面的重新加载。

##### 优点

- 用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染；
- 基于上面一点，`SPA`相对对服务器压力小；
- 前后端职责分离，架构清晰，前端进行交互逻辑，后端负责数据处理；

##### 缺点

- 初次加载耗时多：为实现单页`Web`应用功能及现实效果，需要在加载页面的时候将`Java script`、`CSS`统一加载，部分页面按需加载；
- 前进后退路由管理：由于单页应用在一个页面中现实所有内容，所以不能使用浏览器的前进后退功能，所有的页面切换需要自己建立堆栈管理；
- `SEO`难度较大：由于所有的内容都在一个页面中动态替换显示，所以在`SEO`上有着天然的弱势。

#### **2.怎么理解`Vue`的单向数据流**

所有的`prop`都是使得其父子`prop`之间形成了一个**单向下行绑定：**父级`prop`的更新会向下流动到子组件中，但是反过来则不行。这样可以防止从子组件意外改变父级组件的状态，从而导致应用的数据流向难以理解。

额外的，每次父级组件发生更新时，子组件中所有的`prop`都会刷新到最新的值。这意味着你不应该在一个子组件内部改变`prop`。如果你这样做了，`Vue`会在浏览器的控制台中发出警告。子组件想修改时，只能通过`$emit`派发出一个自定义事件，父组件接收到后，由父组件修改。

有两种常见的试图改变一个`prop`的情形：

- **这个`prop`用来传递一个初始值；这个子组件接下来希望将其作为一个本地的`prop`数据来使用**。这种情况下，最好定义一个本地的`data`属性并将这个`prop`用作其初始值

```javascript 
props: ['initialCounter'],
data: function(){
  return {
    counter: this.initialCounter
  }
} 
```

- 这个`prop`以一种原始的值传入且需要进行转换。在这种情况下，最好使用这个`prop`的值来定义一个计算属性

```javascript
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

####**3.computed和watch的区别和运用的场景**

`computed`：是计算属性，依赖其他属性值，并且`computed`的值有缓存，只有它依赖的属性值发生改变，下一次获取`computed`的值时才会 重新计算`computed`的值；

`watch`：更多的是「观察」的作用，类似于某些数据的监听回调，每当监听的数据发生变化时都会执行回调进行后续的操作；

**运用场景**

- 当我们需要进行数值计算，并且依赖于其他数据时，应该使用`computed`，因为可以利用`computed`的缓存特性，避免每次获取值时，都要重新计算;
- 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用`watch`，使用`watch`选项允许我们执行异步操作（访问一个API）,限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

#### **4.直接给一个数组项赋值，`Vue`能检测到变化吗？**

由于`javascript`的限制，`Vue`不能检测到以下数组的变动：

- 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
- 当你修改数组长度时，例如：`vm.items.length = newLength`

为了解决第一个问题，`Vue`提供了以下操作方法

```javascript 
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// vm.$set, Vue.set的一个别名
vm.$set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(itemOfItem, 1, newValue)
```

为了解决第二个问题，`Vue`提供了以下操作方法

```javascript
// Array.prototype.splice
vm.items.splice(newLength)
```

#### **5.`Vue`的父组件和子组件生命周期钩子函数执行顺序**

`Vue`的父组件和子组件生命周期钩子函数执行顺序可以归类为以下4部分：

- 加载渲染过程

  父`beforeCreate` -> 父`created` -> 父`beforeMount` -> 子`beforeCreate` -> 子`created` -> 

  子`beforeMount` -> 子`mounted` -> 父`mounted`

- 子组件更新过程

  父`beforeUpdate` -> 子`beforeUpdate` -> 子`updated` -> 父`updated`

- 父组件更新过程

  父`beforeUpdate` -> 父`updated`

- 销毁过程

  父`beforeDestroy` -> 子`beforeDestroy` -> 子`destroyed` -> 父`destroyed`

#### **6.在哪个生命周期内调用异步请求?**

可以在钩子函数`created`、`beforeMount`、`mounted`中进行调用，因为在这三个钩子函数中，`data`已经创建，可以将服务端返回的数据进行赋值。但是推荐在`created`钩子函数中调用异步请求，因为在`created`钩子函数中调用异步请求有以下优点：

- 能更快获取到服务端数据，减少页面`loading`事件；
- `ssr`不支持`beforeMount`、`mounted`钩子函数，所以放在`created`中有助于一致性；

#### **7.在什么阶段才能访问操作`DOM`?**

在钩子函数`mounted`被调用前，`Vue`已经将编译好的模版挂载到页面上，所以在`mounted`中可以访问操作`DOM`。

#### **8.父组件可以监听子组件的生命周期吗?**

比如有父组件`Parent`和子组件`Child`，如果父组件监听到子组件挂载`mounted`就做一些逻辑处理，可以通过以下写法实现：

```javascript
// Parent.vue
<Child @mounted="doSomething"/>
  
// Child.vue
mounted(){
	this.$emit("mounted")
}
```

以上需要手动通过`$emit`触发父组件的事件，更简单的方式可以在父组件引用子组件时通过`@hook`来监听即可，如下所示：

```javascript
// Parent.vue
<Child @hook:mounted="doSomething"/>
  
doSomething(){
  console.log('父组件监听到 mounted 钩子函数 ...')
}
  
// Child.vue
mounted(){
  console.log('子组件触发 mounted 钩子函数 ...')
}

// 以上输出顺序为：
// 子组件触发 mounted 钩子函数 ...
// 父组件监听到 mounted 钩子函数 ... 
```

`@hook`方法不仅可以监听`mounted`，其他生命周期事件，例如：`created`，`updated`等都可以监听。

#### **9.谈谈你对`keep-alive`的了解**

`keep-alive`是`Vue`内置的一个组件，可以是包含的组件保留状态，避免重新渲染，其有以下特性：

- 一般结合路由和动态组件一起使用，用于缓存组件；
- 提供`include`和`exclude`属性，两者都支持字符串或正则表达式，`include`表示只有名称匹配的组件会被缓存，`exclude`表示任何名称匹配的组件都不会被缓存，其中`exclude`的优先级比`include`高；
- 对应两个钩子函数`activated`和`deactivated`，当组件被激活时，触发钩子函数`activated`，当组件被移除时，触发钩子函数`deactivated`。

#### **10.`v-model`的原理**

在`vue`项目中主要使用`v-model`指令在表单`input`、`textarea`、`select`等元素上创建双向数据绑定，我们知道`v-model`本质上不过是语法糖，`v-model`在内部为不同的输入元素使用不同的属性并抛出不同的事件：

- `text`和`textarea`元素使用`value`属性和`input`事件；
- `checkbox`和`radio`使用`checked`属性和`change`事件；
- `select`字段将`value`作为`prop`并将`change`作为事件。

以`input`表单元素为例：

```javascript
< input v-model="something" />
// 相当于
<input v-bind:value="something" v-on:input="something = $event.target.value" />
```

如果在自定义组件中，`v-model`默认会利用名为`value`的`prop`和名为`input`的事件，如下所示：

```javascript
// 父组件
<ModelChild v-model="message"></ModelChild>

// 子组件
<div>{{value}}</div>

props:{
	value: String
},
methods: {
  test1(){
    this.$emit('input','小红')
  }
}
```

#### **11.`Vue`组件间通信有哪几种方式**

`Vue`组件间通信主要有：父子组件通信、隔代组件通信、兄弟组件通信

1. `prop / $emit`适用 **父子组件通信**

2. `ref`与 `$parent`/ `$children`适用 **父子组件通信**

   `ref`：如果在普通的`DOM`元素上使用，引用者向的就是`DOM`元素；如果用在子组件上，引用的就是指向组件实例

   `$parent`/ `$children`：访问父 / 子实例

3. `EventBus`（`$emit`/`$on`）适用于**父子，隔代，兄弟组件通信**

   这种方法通过一个空的`Vue`实例作为中央事件总线（事件中心），用它来触发事件和监听事件，从二实现任何组件间的通信，包括父子、隔代、兄弟组件

4. `$attrs`/`$listeners`适用于**隔代组件通信**

   `$attrs`: 包含了父作用域中不被`prop`所设别（且获取）的特性绑定（`class`和`style`除外）。当一个组件没有声明任何`prop`时，这里会包含所有父作用域的绑定（`class`和`style`除外），并且可以通过`v-bind="$attrs"`传入内部组件。通常配合`inheritAttrs`选项一起使用。

   `$listeners`：包含了父作用域中的（不含`.native`修饰器）`v-on`事件监听器。它可以通过`v-on="$listeners"传入内部组件`

5. `provide`/`inject`适用于**隔代组件通信**

   祖先组件中通过`provider`来提供变量，然后在子孙组件中通过`inject`来注入变量。`provide`/`inject API`主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系

6. `Vuex`适用于**父子、隔代、兄弟组件通信**

   `Vuex`是一个专门为`Vue.js`应用程序开发的状态管理模式。每一个`Vuex`应用的核心就是`store`（仓库）。`store`基本就是一个容器，它包含着应用中大部分的状态（`state`）

   -  `Vuex`的状态存储是响应式的。当`Vue`组件从`store`中读取状态的时候，若`store`中的状态发生变化，那么相应的组件也会高效更新。
   - 改变`store`中的状态的唯一途径就是显示地提交（`commit`）`mutation`。这样使得我们可以方便地追踪每一个状态的变化。

#### **12.你用过`Vuex`吗？**

`Vuex`是一个专门为`Vue.js`应用程序开发的状态管理模式。每一个`Vuex`应用的核心就是`store`（仓库）。`store`基本就是一个容器，它包含着应用中大部分的状态（`state`）

1. `Vuex`的状态存储是响应式的。当`Vue`组件从`store`中读取状态的时候，若`store`中的状态发生变化，那么相应的组件也会高效更新。
2. 改变`store`中的状态的唯一途径就是显示地提交（`commit`）`mutation`。这样使得我们可以方便地追踪每一个状态的变化。

主要包含以下几个模块：

- `State`：定义了应用状态的数据结构，可以在这里设置默认的初始状态。
- `Getter`：允许组件从`Store`中获取数据，`mapGetters`辅助函数仅仅是将`store`中的`getter`映射到局部计算属性。
- `Motation`：是唯一更改`store`中状态的方法，且必须是同步函数。
- `Action`：用于提交`mutation`，而不是直接变更状态，可以包含任意异步操作。
- `Module`：允许将单一的`Store`拆分为多个`store`且同时保存在单一的状态中。

#### **13.`Vue`是如何实现数据双向绑定的？**

`Vue`主要通过以下**4**步骤来实现数据双向绑定的：

- 实现一个监听器`Observer`：对数据对象进行遍历，包括子属性对象的属性，利用`Object.defineProperty()`对属性都加上`setter`和`getter`。这样的话，给这个对象的某个值赋值，就会触发`setter`，那么就能监听到了数据变化。
- 实现一个解析器`Compile`：解析`Vue`模板指令，将模版中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。
- 实现一个订阅者`Watcher`：`Watcher`订阅者是`Observer`和`Compile`之间的通信桥梁，主要的任务是订阅`Observer`中的属性值变化的消息，当收到属性变化的消息时，触发解析器`Compile`中对应的更新函数。
- 实现一个订阅器`Dep`：订阅器采用**发布-订阅**设计模式，用来收集订阅者`Watcher`，对监听器`Observer`和订阅者`Watcher`进行统一管理。

#### **14.`Vue`框架怎么实现对象和数组的监听？**

```javascript
/**
 * Observe a list of Array items.
 */
observeArray (items: Array<any>) {
	for (let i = 0, l = items.length; i < l; i++) {
  	observe(items[i])  // observe 功能为监测数据的变化
  }
}

/**
 * 对属性进行递归遍历
 */
 let childOb = !shallow && observe(val) // observe 功能为监测数据的变化
```

`Vue`框架通过遍历数组和递归遍历对象，从而达到利用`Object.difineProperty()`也能对对象和数组（部分方法的操作）进行监听。

#### **15.`Proxy`与`Object.defineProperty`优劣对比**

`Proxy`的优势如下：

- `Proxy`可以直接监听对象和非属性；
- `Proxy`可以直接监听数组的变化；
- `Proxy`有多达13种拦截方法啊，不限于`appley、`ownKeys`、`delectProperty`、`has`等等是`Object.defineProperty`不具备的；
- `Proxy`返回的是一个新对象，我们可以只操作新对象而达到目的，而`Object.defineProperty`只能遍历对象属性直接修改；
- `Proxy`作为新标准受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利。

`Object.defineProperty`的优势如下：

- 兼容性好，支持`IE9`，而`Proxy`存在浏览器兼容性问题，而且无法利用`polyfill`磨平。

#### **15.虚拟`DOM`的实现原理?**

- 用`javascript`对象模拟真实的`DOM`树，对真实的`DOM`进行抽象；
- `diff`算法 -- 比较两颗虚拟`DOM`树的差异；
- `pach`算法 -- 将两个虚拟`DOM`对象的差异应用到真正的`DOM`树

