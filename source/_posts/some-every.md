---
title: some/every
date: 2020-06-03 11:32:07
tags: js
---

## Every 和 Some

```js
//Every
const random_numbers = [13, 2, 37, 18, 5];
const more_random_numbers = [0, -1, 30, 22];
    
const isPositive = (number) => {
	return number > 0;
};

random_numbers.every(isPositive); // returns true
more_random_numbers.every(isPositive); // returns false

random_numbers.every((number) => {
	return number > 0;
});
```

every` 函数返回一个布尔值。如果数组中的所有元素都通过了测试，将会返回 `true` 。哪怕数组中只有一个元素没有通过测试，返回的结果也是 `false

```js
//some
const random_numbers = [ 13, 2, 37, 18, 5 ]
const more_random_numbers = [ 0, -1, 30, 22 ]

const isPositive = (number) => {
	return number > 0
}
    
random_numbers.some(isPositive); // returns true
more_random_numbers.some(isPositive); // returns true

```

`some` 函数测试是否至少有一个数组中的元素通过了测试。

