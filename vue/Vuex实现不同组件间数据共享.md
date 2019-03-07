# Vuex实现不同组件间数据共享

## 概述

https://vuex.vuejs.org/zh/

通过路由来动态加载组件时，组件间切换时，组件间的关系不再是父子、兄弟或非父子的关系。所以数据之间的传递可以通过我们之前介绍的设置动态路由或get传值。但是通过这两种方式传递的数据是有限的。

当然我们还可以通过 `LocalStorage` 和 `SessionStorage` 来实现数据共享。一般小项目中使用这些进行数据共享就够了。

但是对于大型项目中进行数据共享，Vuex(大型项目)是合适的。

Vuex-状态管理插件，可以**解决不同组件间的数据共享/数据持久化**。它是一个专门为 `Vue.js` 应用程序开发的状态管理模式。

* vuex 解决了组件之间同一状态的共事问题（解决了不同组件之间的数据共享）
* 组件里面数据的持久化
 
*注意，小项目中能不用 Vuex 就尽量不用。Vuex 只能用在大型项目中。*

## 使用vuex

基本操作流程

* 安装 vuex `npm install vuex --save`
* `src` 目录下新建 `vuex/store.js` 
* 在刚才创建的 `store.js` 中引入 vue、引入 vuex，并且 use vuex

```js
// store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```
* 定义`State`-数据

```js
// store.js
// 1. state 在 vuex 中用于存储数据（相当于 vue 中 data）
var state = {
    count: 1
}
```

* 定义`Mutations`-方法

```js
// store.js
// 2. mutations 里面放的是方法，方法主要用于改变 state 里面的数据
// 注意如果传值，写法 incCount(state, data)...
var mutations = {
    incCount() {
        ++state.count
    }
}
```

* 实例化 `Vuex.Store`

```js
// store.js
// 实例化 Vuex.store
const store = new Vuex.Store({
    state, // 相当于 state: state
    mutations, // 相当于 mutations: mutations
})
```

* 暴露 `Store`

```js
// store.js
// 暴露
export default store
```

* 在组件中使用

<!-- ![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190307-180859@2x.png) -->


* 不常用的核心方法 `getters / actions`

```js
// store.js
// 3. getters 方法，计算属性，改变 state 里面的 count 数据时会触发 getters 里面的方法，获取新的值
// 不常用
var getters = {
    computedCount: state => {
        return state.count*2
    }
}

// 4. actions 方法，它提交的是 mutations 里面的方法，而不是直接变更状态
// 也就是触发 mutations 里面的方法，然后 mutations 里面的方法改变 state 里面的数据
// 基本没有用

var actions = {
    incMutationsincCount(context) {
        context.commit('incCount')
    } 
}
```

```
<!-- 在组件中 -->
{{this.$store.getters.computedCount}}

this.$store.dispatch('incMutationsincCount')
```

## 总结

vuex-状态管理插件，可以**解决不同组件间的数据共享/数据持久化**。适用于大型项目。

vuex 的使用流程，主要的核心方法是 `state` (存储数据) 和 `mutations`（里面放方法改变 state 里面的数据）。



