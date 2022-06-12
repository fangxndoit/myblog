---
title: css插件
date: 2020-05-14 15:06:26
tags: css
---

####  可选按钮



``` vue
<div id="app">
  <div>
    <span class="label" @click="fn1">
      <i v-show="l1" class="regular"></i>
    </span>
    <div>
    <span class="label" @click="fn2">
      <i v-show="l2" class="regular"></i>
    </span>
  </div>
  </div>
</div>

```

``` js
new Vue({
  el: "#app",
  data: {
    l1: true,
    l2: true
  },
  methods: {
  	fn1(){
    	this.l1 = !this.l1
    },
    fn2(){
    	this.l2 = !this.l2
    }
  }
})
```



``` css
body {
  background: #20262E;
  padding: 20px;
  font-family: Helvetica;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  transition: all 0.2s;
}

.label {
  display: inline-block;
  background-color: #32aeff;
  position: relative;
  width: 16px;
  height: 16px;
  border-radius: 4px;
}
.label:after {
  content: "\00a0";
  display: inline-block;
  border: 1px solid #fff;
  border-top-width: 0;
  border-right-width: 0;
  width: 8px;
  height: 5px;
  -webkit-transform: rotate(-50deg);
  position: absolute;
  top: 3px;
  left: 3px;
}
.label .regular {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: inline-block;
  width: 15px;
  height: 15px;
  border-radius: 4px;
  background-color: #fff;
  border: 1px solid #32aeff;
}
```

通过控制 i.regular 显示隐藏来实现选择与放弃

[在线运行](https://jsfiddle.net/fire_flower/7e0g4jqn/16/)

#### 箭头符号

``` css
.icon_right {
  position: absolute;
  border: solid #888888;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 3px;
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}
```

[在线运行]()

#### 

