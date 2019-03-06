# Vue编程式导航

https://router.vuejs.org/zh/guide/essentials/navigation.html

除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。也就是**通过js实现跳转路由**。

## 第一种跳转方式

```js
this.$router.push('home')
// 或者
this.$router.push({ path: 'home' })
// 或者
this.$router.push({ path: 'home/49' }) // (动态路由)
```

## 第二种跳转方式

命名路由

```js
this.$router.push({ name: 'user', params: { userId: '123' }})
// 或者
this.$router.push({ path: 'register', query: { plan: 'private' }}) //register?plan=private
```

注意：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path`：

```js
const userId = '123'
this.$router.push({ name: 'user', params: { userId }}) // -> /user/123
this.$router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
this.$router.push({ path: '/user', params: { userId }}) // -> /user
```

## `router.go(n)`

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

```js
// 在浏览器记录中前进一步，等同于 history.forward()
this.$router.go(1)

// 后退一步记录，等同于 history.back()
this.$router.go(-1)

// 前进 3 步记录
this.$router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
this.$router.go(-100)
this.$router.go(100)
```