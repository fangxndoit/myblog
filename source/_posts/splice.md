---
title: splice
date: 2020-05-20 17:13:05
tags: js
---

**`splice()`** 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组

## 使用JavaScript Array的`splice()`方法删除元素

``` js
let scores = [1,2,3,4,5,6];
let deletedScores = scores.splice(0, 3);
console.log(scores); 
//  [4, 5]
console.log(deletedScores); 
// [1, 2, 3]
```

![图解](https://cdn.javascripttutorial.net/wp-content/uploads/2016/08/JavaScript-Array-Splice-Delete-Example.png)

```js
Array.splice(position,0,new_element_1,new_element_2,...);
```

- 该`position`参数指定要在其中插入新元素的数组中的起始位置。
- 第二个参数为零（0），指示该`splice()`方法不要删除任何元素。
- 第三个自变量，第四个自变量等是插入到数组中的新元素

## 使用JavaScript Array的`splice()`方法插入元素

```js
let colors = ['red', 'green', 'blue'];
colors.splice(2, 0, 'purple');
console.log(colors); 
// ["red", "green", "purple", "blue"]

colors.splice(1, 0, 'yellow', 'pink');
console.log(colors); 
// ["red", "yellow", "pink", "green", "purple", "blue"]
```

![图解](https://cdn.javascripttutorial.net/wp-content/uploads/2016/08/JavaScript-Array-Splice-Insert-Example.png)

## 使用JavaScript Array的`splice()`方法替换元素

```js
let languages = ['C', 'C++', 'Java', 'JavaScript'];
languages.splice(1, 1, 'Python');
console.log(languages); 
// ["C", "Python", "Java", "JavaScript"]

languages.splice(2,1,'C#','Swift','Go');
console.log(languages); 
// ["C", "Python", "C#", "Swift", "Go", "JavaScript"]
```

![图解](https://cdn.javascripttutorial.net/wp-content/uploads/2016/08/JavaScript-Array-Splice-Replace-Example.png)

