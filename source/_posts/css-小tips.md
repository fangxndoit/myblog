---
title: css 小tips
date: 2022-07-28 14:29:48
tags:
---

#### 图片img占位

```scss
.banner{
	height: 0;
  overflow: hidden;
	padding-boottom: (height/width)*100%;
	//.banner__img
  &__img{
    width: 100%
  }
}
```

#### 字体缩小

```scss
.font{
	font-siez: 20px;
  transform: scale(.5, .5);
  transform-origin: center;
}
```

#### 下载`normalize`

```
npm install normalize.css
```

#### `input`

```css
display: block;
border: none;
outline: none;
background: none;
```

#### 设配

```js
var width = document.documentElement.clientWidth || document.body.clientWidth;
var ratio = width / 375;
var fonstSize = 100 * ratio;
document.getElementsByTagName('html')[0].style['font-size'] = fonstSize + 'px';
```

