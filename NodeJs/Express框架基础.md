# Express 框架基础

## Express 概念

Express 是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架。

Express 框架是**后台的 Node 框架**，所以和 jQuery、zepto、yui、bootstrap 都不一个东西。 Express 在后台的受欢迎的程度类似前端的 jQuery，就是企业的事实上的标准。

**Express 特点**

* Express 是一个基于 Node.js 平台的极简、灵活的 web 应用开发框架，它提供一系列强大的特性，帮助你创建各种 Web 和移动设备应用
* 丰富的 HTTP 快捷方法和任意排列组合的 Connect 中间件，让你创建健壮、友好的 API 变得既快速又简单
* Express 不对 Node.js 已有的特性进行二次抽象，我们只是在它之上扩展了 Web 应用所需的基本功能

中文官网：

http://www.expressjs.com.cn/

http://www.expressjiaocheng.com

**Express 安装使用**

```
npm install express --save
```

```js
//1.引入
var express = require('express'); 
var app = express();

//2.配置路由
app.get('/', function (req, res) { 
    res.send('Hello World!');
});

//3.监听端口
app.listen(3000,'127.0.0.1');
```

## Express 框架中的路由

路由 (Routing) 是由一个 `URI` (或者叫路径) 和一个特定的 `HTTP 方法`(GET、POST 等) 组成的，涉及到应用如何响应客户端对某个网站节点的访问。

**简单的路由配置**

```js
//  get 请求访问一个网址
app.get("网址",function(req,res){});
```

```js
// post 访问一个网址
app.post("网址",function(req,res){});
```

```js
// user 节点接受 PUT 请求
app.put('/user', function (req, res) {
    res.send('Got a PUT request at /user'); 
});
```

```js
// user 节点接受 DELETE 请求
app.delete('/user', function (req, res) {
    res.send('Got a DELETE request at /user'); 
});
```

**动态路由配置**

```js
app.get( '/user/:id',function(req,res){ 
    var id = req.params["id"];
    res.send(id);
});
```

**路由里面获取 Get 传值**

```js
// /news?id=2&sex=nan
app.get('/news', function(req, res) { 
    console.log(req.query);
    // {id: '2', sex: 'nan'}
});
```

## Express 框架中 ejs 的安装使用

**Express 中 ejs 的安装**
```
npm install ejs --save
or
npm install ejs --save-dev
```

**Express 中 ejs 的使用**

```js
var express = require("express");
var app = express();

app.set("view engine","ejs");
// 默认模板位置在 views
app.get("/",function(req,res){ 
    res.render("news",{
        "news" : ["我是小新闻啊","我也是啊","哈哈哈哈"]
    });
});

app.listen(3000);
```

指定模板位置

```js
app.set('views', __dirname + '/views');
```

`.ejs` 文件中

```html
<!-- 引入模版 -->
<%- include header.ejs%>
```

```html
<!-- 绑定数据 -->
<%=h%>
```

```html
<!-- 绑定 html 数据 -->
<%-h%>
```

```html
<!-- 判断语句 -->
<%if(true){%>
    <div></div>
<%}else{%>
    <div></div>
<%}%>
```

```html
<!-- 循环数据 -->
<%for(var i = 0; i < list.length; i++) {%>
    <li><%=list[i]%></li>
<%}%>
```

## 利用 Express.static 托管静态文件

 1、如果你的静态资源存放在多个目录下面，你可以多次调用 `express.static` 中间件:

 ```js
app.use(express.static('public'));
 ```

 2、如果你希望所有通过 `express.static` 访问的文件都存放在一个“虚拟(virtual)”目录(即目录根本不存在)下面，可以通过为静态资源目录指定一个挂载路径的方式来实现，如下所示:

 ```js
app.use('/static', express.static('public'));
// 可以通过带有 “/static” 前缀的地址来访问 public 目录下 面的文件了
 ```

## Express 中间件

Express 是一个自身功能极简，完全是由**路由**和**中间件**构成一个的 web 开发框架:从
本质上来说，一个 Express 应用就是在调用各种中间件。

中间件(**Middleware**) 是一个函数，它可以访问请求对象(request object (req)), 响应对象(response object (res)), 和 web 应用中处理请求-响应循环流程中的中间件，一般被命名为 next 的变量。

**中间件的功能包括**:

* 执行任何代码
* 修改请求和响应对象
* 终结请求-响应循环
* 调用堆栈中的下一个中间件

注意，如果我的 get、post 回调函数中，没有 next 参数，那么就匹配上第一个路由，就不会往下匹配了。如果想往下匹配的话，那么需要写 `next()`

Express 应用可使用如下几种中间件

* 应用级中间件
* 路由级中间件
* 错误处理中间件
* 内置中间件
* 第三方中间件

**应用级中间件**

```js
app.use(function(req,res,next){ /*匹配任何路由*/
    //res.send('中间件'); 
    console.log(new Date());
    next(); /*表示匹配完成这个中间件以后程序继续向下执行*/ 
})
```

**路由级中间件**

```js
app.get("/",function(req,res,next){ 
    console.log("1");
    next();
});
app.get("/",function(req,res){ 
    console.log("2");
});
```

**错误处理中间件**

```js
app.get('/index',function(req,res){
    res.send('首页'); 
})

/*中间件相应 404*/
app.use(function(req,res){ 
    //res.render('404',{}); 
    res.status(404).render('404',{});
})
```

**内置中间件**

```js
//静态服务 index.html 
app.use('/static',express.static("./static")); /*匹配所有的路径*/

app.use('/news',express.static("./static")); /*匹配所有的路径*/
```

**第三方中间件**

```js
// body-parser 中间件 第三方的 获取 post 提交的数据
// 1.cnpm install body-parser --save
// 2.var bodyParser = require('body-parser')
// 3.设置中间件
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json()); //提交的 json 数据的数据

// 4.req.body 获取数据
```

## 获取 Get Post 请求的参数

*  GET 请求的参数在 URL 中，在原生 Node 中，需要使用 url 模块来识别参数字符串。在
Express 中，不需要使用 url 模块了。可以直接使用 `req.query` 对象
* POST 请求在 express 中不能直接获得，可以使用 `body-parser` 第三方中间件模块。使用后，将可以用
`req.body` 得到参数。但是如果表单中含有文件上传，那么还是需要使用 `formidable` 模块

