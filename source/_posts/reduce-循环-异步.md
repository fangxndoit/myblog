---
title: reduce 循环+异步
date: 2022-06-06 16:36:55
tags:
---

####**reduce**

```js
function testPromise(time){
  return new Promise((resolve,reject)=>{
    setTimeout(()=>{
      console.log(`Processing ${time}`)
      resolve(time)
    }, time)
  })
}

let result = [3000,2000,1000, 4000].reduce((acc,nextId)=>{
  return acc.then(()=>{
    return testPromise(nextId)
  })
}, Promise.resolve())

result.then(e=>{
  console.log("All Promises Resolved !!✨")
})
```



