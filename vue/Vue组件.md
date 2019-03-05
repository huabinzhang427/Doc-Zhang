# Vue 组件

## 目录结构

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-133839@2x.png)

和 `App.vue` 同级的有 `assets`-静态资源，`components`-组件, `model`-模块(JS的封装)，`main.js` 是全局的入口文件。

## 组件概念 

在 Vue 中，所有的页面都由组件组成。组件中有模版(`template`)、业务逻辑(`script`) 和 `css`，也就是组件中包含了 html、css 和 js。

Vue 组件的概念就是把页面中公共的功能单独封装成一个组件。需要的页面直接(用标签)进行引入即可。**组件可以扩展 HTML 元素，封装可重用的代码。**

`App.vue` 是一个根组件。下面所有的页面都直接或间接 `挂载` 到根组件上。

## 注册/使用组件

### 局部注册

在我们的开发目录下，新建一个文件夹命名为 `components`，这里面专门放我们的组件。在该文件夹下创建组件文件 `*.vue`(首字母大写如 `App.vue`)。 

示例代码

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-121348%402x.png)


然后回到需要使用的组件中，进行手动添加（后面会说到可以使用 `路由-router` 进行动态添加）

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-121453@2x.png)

我们在js代码中引入组件

```js
// 在模块系统中局部注册
import Home from './components/Home.vue' 
import News from './components/News.vue'
```
就相当于

```js
var Home = { /* ./components/Home.vue里的代码 */ }
var News = { /* ./components/News.vue里的代码 */ }
```
然后在 `components` 选项中定义想要使用的组件：

```js
...
components: {
    'v-home': Home, // 注册组件
    'v-news': News,
  }
//   属性名为使用该组件的标签(注意，不要与原生标签相同)，属性值为定义的该组件对象
```

在组件使用中，组件样式可能出现互相干扰。举个例子

在 Home 组件中定义了样式

```css
<style lang='scss'>
    h2{
        color: red;
    }
</style>
```

将 Home 组件引入 App 组件，该样式使得 App 组件中的 `h2` 标签样式也发生了改变。有两种方法可以解决该问题：

在 Home 组件中的样式里如下设置

```css
/* 添加 scoped */
<style lang='scss' scoped>
    h2{
        color: red;
    }
</style>
```

或者在 Home 组件的 `<template>` 下的根节点添加一个 `id` 属性，在样式中设置该 `id` 属性的样式，设置`局部作用域`

```css
<style lang='scss' scoped>
#home{
    h2{
        color: red;
    }
}
</style>
```

## 总结

重点，在模块系统中局部注册。步骤(Home 可任意命名代替)：

* 在 `components` 文件夹中创建 `Home.vue` 的组件
* 在需要引入的组件js中引入

```js
import Home from './components/Home.vue' 
```
* 注册（挂载）组件

```js
...
components: {
    'v-home': Home, // 注册组件
}
```
* 在 `template` 下的子节点中使用该组件标签

```html
<template>
  <div id="app">
    <!-- 使用组件 -->
    <v-home></v-home>
  </div>
</template>
```

