# Vue-Router路由

https://router.vuejs.org/zh/installation.html

路由可以让我们动态的往根组件中挂载不同的组件(页面)，而不是手动设置去挂载。

## `vue-router`路由配置

* 安装

```
cnpm install vue-router -S
```

* 引入并使用 (`main.js`)

```js
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

* 配置路由(`main.js`)

```js
// 1. 创建组件，引入组件
import Home from './components/Home.vue'
import News from './components/News.vue'

// 2. 定义路由
const routes = [
  { path: '/home', component: Home },
  { path: '/news', component: News },
  { path: '*', redirect: '/home' }, // 通过重定向配置跳转
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')

```
* 在根组件的 `<template>` 中

```html

<!-- 使用 router-link 组件来导航. -->
<!-- 通过传入 `to` 属性指定链接. -->
<!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
<router-link to="/home">首页</router-link>
<router-link to="/news">新闻</router-link>


<!-- 路由出口 -->
<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

## 动态路由传值和get传值

### 动态路由传值

定义路由的方式

```js
// 设置传递参数 aid
{ path: '/content/:aid', component: Content },/
```

在组件中设置跳转链接，绑定传递数据

```html
<ul>
    <li v-for="(item, index) in list">
        <router-link :to="'/content/'+index">{item}}</router-link>
    </li>
</ul>
```

对应传递的数据会设置到 `$route.params`，在跳转到的组件中取出数据

```js
this.$route.params
// {aid: "1"}
```

### get传值

定义路由的方式和普通定义路由的方式相同。

在组件中设置跳转链接，绑定传递数据

```html
<ul>
    li v-for="(item, index) in list">
        <router-link :to="'/product?index='+index">{{item}}</router-link>
    </li>
</ul>
```

对应传递的数据会设置到 `$route.query`，在跳转到的组件中取出数据

```js
this.$route.query
// {index: "1"}
```

## 总结

* 路由配置
* 动态路由和get传值