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

