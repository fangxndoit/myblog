---
title: sleep
date: 2020-07-22 20:02:26
tags: js
---

```js
// Promise
const sleep = (time) => {
  return new Promise(resolve => setTimeout(resolve, time))
}
sleep(1000).then(()=>{
  //其他操作
})

//Generator
function* sleepGenerator(time){
	yield new Promise(function(resolve,reject){
    setTimeout(resolve,time)
  })
}
sleepGenerator(1000).next().value.then(()=>{
  //其他操作
})

//async
function sleep(time){
  return new Promise(resolve => setTimeout, time)
}
async function output(){
  let out = await sleep(1000);
  return out;
}
output();

//ES5
function sleep(callback,time){
  if(typeof callback === 'function'){
     setTimeout(callback,time)
	}
}
function output(){
  //其他操作
}
sleep(output,1000)
```

