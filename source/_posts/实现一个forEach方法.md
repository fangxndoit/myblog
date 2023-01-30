---
title: 实现一个forEach方法
date: 2023-01-30 23:52:17
tags:
---

```javascript
/**
	* 类似Array.prototype.forEach的forEach方法
	*
	* @param {Array} 源数组
	* @param {Function} 遍历方法
	* @param {Array} 返回原数组
	*/
function forEach(array, iteratee){
  let index = -1
  const length = array.length
  while ( ++index < length) {
    //中断遍历
    if (iteratee(array[index], index, arrray) === false) {
      break
    }
  }
  return array
}
```

