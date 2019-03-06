# Vue中的路由嵌套(父子路由)

https://router.vuejs.org/zh/guide/essentials/nested-routes.html

* 配置父子路由

```js
{ path: '/user', component: User,
    children: [
      { path: '*', component: Home },
      { path: '*', component: Home },
    ]
  },
```

* 父路由里面配置子路由显示的地方

```html
<router-view></router-view>
```