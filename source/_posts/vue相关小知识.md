---
title: vue小知识 
tags: vue
grammar_cjkRuby: true
---

##### 不要在同个元素上同时使用 <font color=red>v-if</font> 和 <font color=red>v-if</font> 指令

为了过滤数组中的元素，我们很容易将v-if与v-for在同个元素同时使用

``` bash
// 不好的做法
<div v-for='product in products' v-if='product.price < 500'>
```

问题是在 Vue 优先使用v-for指令，而不是v-if指令。它循环遍历每个元素，然后检查v-if条件。
``` bash
this.products.map(function (product) {
  if (product.price < 500) {
    return product
  }
})
```
这意味着，即使我们只想渲染列表中的几个元素，也必须遍历整个数组。

这对我们来当然没有任何好处。

一个更聪明的解决方案是遍历一个计算属性，可以把上面的例子重构成下面这样的：

``` bash
<div v-for='product in cheapProducts'>
 
computed: {
  cheapProducts: () => {
    return this.products.filter(function (product) {
      return product.price < 100
    })
  }
}
```
#### 这么做有几个好处：

* 渲染效率更高，因为我们不会遍历所有元素

* 仅当依赖项更改时，才会重使用过滤后的列表

* 这写法有助于将组件逻辑从模板中分离出来，使组件更具可读性

