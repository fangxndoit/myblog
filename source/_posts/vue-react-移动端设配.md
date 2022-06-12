---
title: vue/react 移动端设配
date: 2020-03-26 15:31:53
tags:
---

#### px2rem或postcss-px2rem
* 在移动端中，为了设配不同的设备，通常使用rem来做适配。
* rem是通过根元素进行适配的，网页中的根元素指的是<html>，我们通过设置<html>的字体大小就可以控制 rem 的大小（1rem = 1根元素字体大小）。
* 可见，只要我们根据不同屏幕（使用css媒体查询或js）设定好根元素<html>的字体大小，其他已经使用了rem单位的元素就会自适应显示相应的尺寸。
* 设计稿一般是按照一种特定设备型号（如iphone6）为基础且以px单位来定义样式，为了让设计稿能够通用在不同的设备型号中，则存在着从px到rem的繁琐计算转化过程，因 此需要更加科学的方式来使用rem单位。
* px2rem或postcss-px2rem的原理：将css中px编译为rem，配合js根据不同手机型号计算出dpr的值，修改<meta>的viewport值和置<html>的font-size

#### recat项目配置postcss-px2rem
* 使用yarn 安装项目所需依赖后，安装 lib-flexible 、 postcss-px2rem 和 postcss-loader
``` bash
  yarn add postcss-px2rem lib-flexible 
  yarn add postcss-loader --dev
```
* 在入口页面 index.html 中设置<meta>标签
``` bash
  <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
```
* 然后在项目入口文件 index.js 中引入 lib-flexible
``` bash
import 'lib-flexible';
```
* 暴露webpack配置
``` bash
  yarn eject
```
* 在项目config目录下的 webpack.config.js 中引入 postcss-px2rem
``` bash
const px2rem = require('postcss-px2rem')
```
* 在 webpack.config.js 的 postcss-loader loader里面添加 
``` bash
{
        loader: require.resolve('postcss-loader'),
        options: {
          /* 省略代码... */
          plugins: () => [
            require('postcss-flexbugs-fixes'),
            require('postcss-preset-env')({
              autoprefixer: {
                flexbox: 'no-2009',
              },
              stage: 3,
            }),
            px2rem({remUnit: 37.5}), // 添加的内容
            /* 省略代码... */
          ],
          sourceMap: isEnvProduction && shouldUseSourceMap,
        },
      },
```
* 使用 yarn start 重启项目，完成配置

#### vue项目配置px2rem
* 使用yarn 安装项目所需依赖后，安装 lib-flexible 、 postcss-px2rem 和 postcss-loader
``` bash
  yarn add postcss-pxtorem lib-flexible 
  yarn add amfe-flexible --dev
  yarn add autoprefixer --dev
```
* 在入口页面 index.html 中设置<meta>标签
``` bash
  <meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
```
* 然后在项目入口文件 index.js 中引入 lib-flexible
``` bash
import 'amfe-flexible';
```
* 在vue.config.js中 添加以下代码
``` bash 
const autoprefixer = require('autoprefixer')
const pxtorem = require('postcss-pxtorem')
const path = require('path')

module.exports = {
  css: {
    loaderOptions: {
      postcss: {
        plugins: [
          autoprefixer(),
          pxtorem({
            rootValue: 37.5,
            propList: ['*']
            // selectorBlackList: ['van']
          })
        ]
      }
    }
  }
}
```
#### 适用情况 & 不适用情况
* 以上实现转换适用于
(1)vue 组件中编写的<style></style>下的css。
(2)从 react 项目的 index.js 或者 vue 项目的 main.js 中通过import ‘../../static/css/reset.css’引入css。
(3)在 vue 组件的<script type=”text/ecmascript-6″>import ‘../../static/css/reset.css'</script>中引入css。
* 另外的情况不适用
(1)在 vue 组件的<style></style>中通过@import “../../static/css/reset.css (可考虑上面（2）、（3）的形式引入)。
(2)外部样式：<link rel=”stylesheet” href=”static/css/reset.css”>。
(3)元素内部样式：style="height: 417px; width: 550px;"。

