# Vue请求网络数据

Vue 中请求数据的模块有很多，包括 `vue-resource`-官方提供的一个Vue插件、`axios`、`fetch-jsonp`等

## 使用官方 `vue-resource`

https://github.com/pagekit/vue-resource

### 安装

指令 `cnpm install vue-resource -S`

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-155533@2x.png)

注意，指令中的 `-S(--save)` 是把插件写入到 `package.json`，**安装模块操作的时候，必须加上！！！**。如果不加，就不回写入到 `package.json`，当把项目发给别人时会出现问题。

### 配置 `main.js` 

在 `main.js` 中导入插件，并使用 `Vue.use()` 方法使用插件。 Vue 的插件操作必须这么玩。

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-160530@2x.png)

注意，在导入插件后面的名称必须为模块的名称！！！


### 在组件中使用

```js
// global Vue object
Vue.http.get('/someUrl', [config]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [config]).then(successCallback, errorCallback);

// in a Vue instance
this.$http.get('/someUrl', [config]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [config]).then(successCallback, errorCallback);
```

官方示例：

```js
{
  // GET /someUrl
  this.$http.get('/someUrl').then(response => {

    // get body data
    this.someData = response.body;

  }, response => {
    // error callback
  });
}
```

```js
{
  // POST /someUrl
  this.$http.post('/someUrl', {foo: 'bar'}).then(response => {

    // get status
    response.status;

    // get status text
    response.statusText;

    // get 'Expires' header
    response.headers.get('Expires');

    // get body data
    this.someData = response.body;

  }, response => {
    // error callback
  });
}
```

https://github.com/pagekit/vue-resource/blob/develop/docs/http.md

代码演示

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-163403@2x.png)

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-164356@2x.png)

## 第三方模块 `axios`

https://github.com/axios/axios

请求数据的步骤


### 安装


```
$ npm install axios -S
```

### 使用

哪里使用，在哪里引入（注意，`vue-resource`可以全局引入）

```js
import Axios from 'axios'
...
Axios.get(api).then(response => {
    
}).catch(error => {

})
```

## 总结

使用 `vue-resource` 请求数据的步骤

* 在项目中安装 `vue-resource` 模块，注意指令后加上 `-S` 或者 `--save`

```
cnpm install vue-response -S 

或者

cnpm install vue-response --save
```

* `main.js` 中引入 `vue-resource`

```js
import VueResource from 'vue-resource'
Vue.use(VueResource)
```

* 在组件中直接使用

```js
this.$http.get('/someUrl', [config]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [config]).then(successCallback, errorCallback);
```

