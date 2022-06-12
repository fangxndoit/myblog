---
title: 常见css
date: 2020-05-11 10:26:37
tags: css
---

#### 改变输入框内提示文字颜色

``` css
::-webkit-input-placeholder{ /* WebKit browsers */
  color: #999;
}
:-moz-placeholder{ /* Mozilla Firefox 4 to 18 */
  color: #999;
}
::-moz-placeholder{ /* Mozilla Firefox 19+ */
  color: #999;
}
:-ms-input-placeholder{ /* Internet Explorer 10+ */
  color: #999;
}
input:focus::-webkit-input-placeholder{
  color:#999;
}
```

#### 多行省略

``` css
.ellipsis{
  display: box !important;
  display: -webkit-box !important;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 4;/*第几行出现省略号*/
}
```

#### 移动端html标签几个体验优化

```css
html,body{

overflow: hidden;/*手机上写overflow-x:hidden;会有兼容性问题，如果子级如果是绝对定位有运动到屏幕外的话ios7系统会出现留白*/

-webkit-overflow-scrolling:touch;/*流畅滚动,ios7下会有滑一下滑不动的情况，所以需要写上*/

position:realtive;/*直接子级如果是绝对定位有运动到屏幕外的话，会出现留白*/
}
```