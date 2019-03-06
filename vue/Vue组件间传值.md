# Vue组件间传值

## 父组件给子组件传值

理解父子组件的概念，与传值。

### 父组件给子组件传值步骤

* 父组件调用子组件的时候，绑定动态属性

```html
<v-header :title="title"></v-header>
```

* 在子组件里面通过 `props` 接收父组件传过来的数据

```js
props: ['title']
```

注意，当子组件中也有 title 定义的数据时，子组件的调用的 title 显示的是父组件传过来的值，覆盖掉子组件本身定义的 title 数据。同时编译器会报警显示 `[Vue warn]: The data property "title" is already declared as a prop. Use prop default value instead.`


同时父组件不止可以传递值，还可以传递方法。

**传递方法**

将父组件中定义的 `run` 方法传递给子组件

```html
<v-header :title="title" :run='run'></v-header>
```

子组件接收 `run` 方法，直接调用 `run()` 方法执行。这是**子组件指向父组件中的方法！**

```js
props: ['title', 'run']
```

**传递父组件的实例到子组件**

```html
<v-header :home='this'></v-header>
```

子组件中接收

```js
props: ['home'],
```
子组件获取到父组件的实例后，就可以获取父组件的所有数据了。

我们也可以对父组件传递过来的数据进行验证

### `props` 验证

子组件在接收父组件的传递过来的数据时，如果不进行验证就是下面的方法

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

如果要进行验证

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
// 这些属性和值分别是 prop 各自的名称和类型
```
这不仅为你的组件提供了文档，还会在它们遇到错误的类型时从浏览器的 JavaScript 控制台提示用户。比如 `vue.esm.js?290e:628 [Vue warn]: Invalid prop: type check failed for prop "title". Expected Number with value NaN, got String with value "首页".`


## 父组件主动获取子组件的数据和方法

* 父组件中调用子组件的时候定义一个 ref

```html
<v-header ref="header"></v-header>
```

* 在父组件里面通过

```js
this.$refs.header.属性
this.$refs.header.方法
```

## 子组件主动获取父组件的数据和方法

```js
this.$parent.数据
this.$parent.方法
```

## 非父子组件间传值

我们可以使用上面父子组件的形式来解决非父子组件的传值，但过程比较麻烦。

另一种方法

* 新建一个 js 文件，然后引入 vue，实例化 vue，最后暴露这个示例

```js
// VueEvent.js
import Vue from 'vue'

var VueEvent = new Vue()

export default VueEvent
```

* 在要广播的组件中引入刚才定义的实例，通过 `VueEvent.$emit('名称', '数据')` 广播数据

```js
// 广播数据
VueEvent.$emit('home', this.title)
```

* 在接收数据的组件中通过 `VueEvent.$on('home', function(data){})` 接收广播的数据

```js
// 在组件的 mounted() {} 生命周期函数中
VueEvent.$on('home', data => {
            console.log(data)
        })
```

## 总结

父子组件间三种不同的传值方法

### 父组件给子组件传值

* 父组件调用子组件的时候，绑定动态属性

```html
<v-header :title="title"></v-header>
```

* 在子组件里面通过 `props` 接收父组件传过来的数据

```js
props: ['title']
```

* 直接在子组件中使用

### prop 验证

在子组件中

```js
props{
    prop名称: prop类型
}
```

### 父组件主动获取子组件的数据和方法

* 父组件中调用子组件的时候定义一个 ref

```html
<v-header ref="header"></v-header>
```

* 在父组件里面通过

```js
this.$refs.header.属性
this.$refs.header.方法
```

### 子组件主动获取父组件的数据和方法

```js
this.$parent.数据
this.$parent.方法
```

### 非父子组件间传值

* 新建一个 js 文件，然后引入 vue，实例化 vue，最后暴露这个示例

```js
// VueEvent.js
import Vue from 'vue'

var VueEvent = new Vue()

export default VueEvent
```

* 在要广播的组件中引入刚才定义的实例，通过 `VueEvent.$emit('名称', '数据')` 广播数据

```js
// 广播数据
VueEvent.$emit('home', this.title)
```

* 在接收数据的组件中通过 `VueEvent.$on('home', function(data){})` 接收广播的数据

```js
// 在组件的 mounted() {} 生命周期函数中
VueEvent.$on('home', data => {
            console.log(data)
        })
```