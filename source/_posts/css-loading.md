---
title: css loading
date: 2020-04-17 15:26:59
tags: css
---

#### HTML

```html
<div class="donut">
</div>
```

#### CSS

``` css
.donut{
  display: inline-block;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #7983ff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: donut-spin 1.2s linear infinite;
}
@keyframes donut-spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

[在线运行](https://jsfiddle.net/fire_flower/0vnfL4eo/)

#### HTML

```html
<div class='mask'>
  <div class='bouncing-loader'>
    <div></div>
    <div></div>
    <div></div>
  </div>
</div>
```



```css
.mask{
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-color: rgba(255, 255, 255, 0.25);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
}
.bouncing-loader{
	display: flex;
  justify-content: center;
}

.bouncing-loader > div {
	width: 1rem;
  height: 1rem;
  border-radius: 1rem;
  background: #8385aa;
  margin: 3rem .2rem;
  animation: bouncing-loader 0.6s infinite alternate;
}

.bouncing-loader > div:nth-child(2){
	animation-delay: 0.2s;
}
.bouncing-loader > div:nth-child(3){
	animation-delay: 0.4s;
}

@keyframes bouncing-loader{
	to {
		opacity: 0.1;
    transform: translate3d(0, -1rem, 0);
	}
}

```

[在线运行](https://jsfiddle.net/fire_flower/5wo6maup/)

