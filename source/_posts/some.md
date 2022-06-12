---
title: some
date: 2020-05-19 14:19:04
tags: js
---

##### **some()** 方法测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回的是一个Boolean类型的值

例子

```js
let marks = [ 4, 5, 7, 9, 10, 3 ];
```

使用普通的for循环

``` js
let marks = [ 4, 5, 7, 9, 10, 3 ];
let lessThanFive = false;
for (let index=0;index<marks.length;index++){
  if(marks[index]<5){
    lessThanFive = true;
    break;
  }
}
console.log(lessThanFive);
// true
```

使用some

``` js
let marks = [ 4, 5, 7, 9, 10, 3 ];
lessThanFive = marks.some(function(e){
  return e < 5;
})
console.log(lessThanFive);
// true
```

or

``` js
let marks = [ 4, 5, 7, 9, 10, 3 ];
let lessThanFive = marks.some(e => e < 5);
console.log(lessThanFive);
```

### 检查数组中是否存在元素

以下`exists()`函数使用该`some()`方法检查数组中是否存在值：

``` js
function exists(value, array){
  return array.some(e=>e===value);
}
let marks = [4, 5, 7, 9, 10, 2];
console.log(exists(4, marks));
console.log(exists(11, marks));
//true
//false
```

### 检查数组中是否有一个元素在范围内

下面的示例演示如何检查`marks`数组中的任何数字是否在（8，10）范围内：

```js
let marks = [4, 5, 7, 9, 10, 2];
const range = {
  min: 8,
  max: 10
};
let result = marks.some(function(e){
  return e >= this.min && e<= this.max
},range);
console.log(result);
//true
```

## 警告：空数组

如果`some()`在空数组上调用该方法，则无论任何条件如何，结果始终是`false`

```js

let result = [].some(e=>e>0);
console.log(result);
//false

result = [].some(e => e <= 0);
console.log(result);
//false
```

