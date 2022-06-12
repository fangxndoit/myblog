---
title: js并集、交集、差集
date: 2021-03-11 00:44:50
tags:
---

#### **`ES7`&`Array.prototype.includes`**

```javascript
// 并集
let union = function(a,b){
	return [...a,...(b.filter(v=>!a.includes(v)))]
}
// 交集
let intersection = function(a,b){
	return [...(a.filter(v=>b.includes(v)))]
}
// 差集
let difference = function(a,b){
  return [...a,...b].filter(v=>!a.includes(v)||!b.includes(v))
}
```

#### **`ES6`&`Set`**

```javascript
// 并集
let union = function(a,b){
  return Array.from(new Set([...a,...b]))
}
// 交集
let intersection = function(a,b){
  return Array.from(a.filter(v=>(new Set(b)).has(v)))
}
// 差集
let difference = function(a,b){
	return Array.from([...a,...b].filter(v=>!(new Set(a)).has(v)||!(new Set(b)).has(v)))
}
```

#### **`ES5`&`indexOf`**

```javascript
// 并集
var union = function(a,b){
  return a.concat(b.filter(function(v){
  	return a.indexOf(v) === -1
	}))
}
// 交集
var intersection = function(a,b){
	return a.filter(function(v){
  	return b.indexOf(v) > -1
	})
}
// 差集
var difference = function(a,b){
  return a.concat(b).filter(function(v){
    return a.indexOf(v) === -1 || b.indexOf(v) === -1
  })
}

```

