---
title: vue-router 路由钩子
date: 2022-06-10 10:46:50
tags:
---

#### 全局路由钩子

- `router.beforeEach` 全局前置守卫 进入路由之前
- `router.beforeResolve` 全局解析守卫 在`beforeRouteEnter`调用之后凋也
- `router.afterEach ` 全局后置钩子 进入路由之后

```js
// main.js
import router from './router';
router.beforeEach((to, from, next) => {
  next();
})
router.beforeResolve((to, form, next) => {
  next();
})
router.afterEach((to, from, next) => {
  console.log('全局后置钩子');
})
```

##### `to`,`from`,`next`参数

`to`: 将要进入的路由对象

`from`：将要离开的路由对象

`next:Function`进入路由，必须调用 `next()` 

#### 路由独享守卫

- `beforeEnter` 可以为某些路由单独配置守卫

```js
export default [
  {
    path: '/',
    name: 'home',
    component: login,
    beforeEnter: (to, from, next) => {
      console.log('即将进入路由');
      next();
    }
  }
]
```

#### 组件内钩子

- `beforeRouteUpdate` 路由复用同一个组件时
- `beforeRouteEnter` 进入路由前
- `beforeRouteLeave` 离开当前路由

`beforeRouteEnter`组件内访问不到`this`因为钩子在组件实例还没被创建的时候调用，需要传一个回调给`next`来访问

```js
beforeRouteEnter(to, from, next){
  next(vm => {
    //通过 `vm` 访问组件实例`this` 执行回调的时机在mounted后面，
  })
}
```

`beforeRouteLeave` 导航离开该组件对应的路由时调用，eg: 禁止用户离开，或者在离开前做一些操作 清除定时器

```js
beforeRouteLeave(to, from, next){
  if(flag){
    next();
  }else{
    next(false); //取消离开
  }
}
```

#### 完整路由导航解析流程

1. 触发进入其他路由
2. 调用要离开路由的组件守卫`beforeRouteLeave`
3. 调用全局前置守卫`beforeEach`
4. 在重用的组件里调用`beforeRouteUpdate`
5. 调用路由独享守卫`beforeEnter`
6. 解析异步路由组件
7. 在将要进入的路由组件中调用`beforeRouteEnter`
8. 调用全局解析守卫`beforeResolve`
9. 导航被确认
10. 调用全局后置钩子的`afterEach`
11. 触发DOM更新`mounted`
12. 执行`beforeRouteEnter`守卫中传给`next`的回调函数

#### 触发钩子的完整顺序

将路由导航、`keep-alvie`、和组件生命周期钩子结合起来，触发顺序，假设是从a组件离开，第一次进入b组件

1. `beforeRouteLeave`路由组件的组件离开路由前钩子，可取消路由离开
2. `beforeEach`路由全局前置守卫，可用于登录验证、全局路由loading等
3. `beforeEnter`路由独享守卫
4. `beforeRouteEnter`路由组件的组件进入路由前钩子
5. `beforeResolve`路由全局解析守卫
6. `afterEach`路由全局后置钩子
7. `beforeCreate`组件生命周期，不能访问`this`
8. `created`组件生命周期，可以访问`this`，不能访问dom
9. `beforeMount`组件生命周期
10. `deactivated`离开缓存组件a，或者触发a的`beforeDestory`和`destroyed`组件销毁钩子
11. `mounted`访问/操作dom
12. `activated`进入缓存组件，进入a的嵌套子组件
13. 执行`beforeRouteEnter`回调函数`next`

