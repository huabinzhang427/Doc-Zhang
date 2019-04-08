# Express路由模块化

http://www.expressjs.com.cn/guide/routing.html

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/NodeJs/images/WX20190408-194247@2x.png)



## `express.Router`

使用 express.Router 该类创建**模块化**，**可安装**的路由处理程序。**一个 Router 实例是一个完整的中间件和路由系统**。

在单独的模块化路由文件中加载中间件功能，定义一些路由，并将路由器模块安装在主应用程序中的路径上。

```js
// birds.js
var express = require('express')
var router = express.Router()

// middleware
router.use(function timeLog (req, res, next) {
  console.log('Time: ', Date.now())
  next()
})

// define the home page route
router.get('/', function (req, res) {
  res.send('Birds home page')
})
// define the about route
router.get('/about', function (req, res) {
  res.send('About birds')
})

module.exports = router
```

在应用程序中加载路由器模块

```js
var birds = require('./birds')
// ...
app.use('/birds', birds)
```
该应用程序现在能够处理请求 `/birds` 和 `/birds/about`，以及调用 `timeLog` 中间件功能是特定的路线。


## 使用

创建文件 `routes/admin.js` 单独的模块化路由

```js
var express=require('express');

var router = express.Router();

var login=require('./admin/login.js');
var product=require('./admin/product.js');
var user=require('./admin/user.js');

router.use('/login',login);
router.use('/product',product);
router.use('/user',user);

module.exports = router;
```

创建文件 `routes/admin/login.js`

```js
var express=require('express');

var router = express.Router();   /*可使用 express.Router 类创建模块化、可挂载的路由句柄*/

router.get('/',function(req,res){
    res.send('登录页面');

})
//处理登录的业务逻辑
router.post('/doLogin',function(req,res){
    res.send('admin user');

})

module.exports = router;   /*暴露这个 router模块*/
```

创建文件 `routes/admin/product.js`

```js
var express=require('express');

var router = express.Router();   /*可使用 express.Router 类创建模块化、可挂载的路由句柄*/

router.get('/',function(req,res){
    res.send('显示商品首页');

})
//处理登录的业务逻辑
router.get('/add',function(req,res){
    res.send('显示商品 增加111');

})
router.get('/edit',function(req,res){
    res.send('显示商品 修改');

})
router.get('/delete',function(req,res){
    res.send('显示商品 删除');

})

module.exports = router;   /*暴露这个 router模块*/
```

创建文件 `routes/admin/user.js`

```js
var express=require('express');

var router = express.Router();   /*可使用 express.Router 类创建模块化、可挂载的路由句柄*/

router.get('/',function(req,res){
    res.send('显示用户首页');

})
//处理登录的业务逻辑
router.get('/add',function(req,res){
    res.send('显示增加用户');

})

module.exports = router;   /*暴露这个 router模块*/
```

在 `app.js` 中加载

```js
var express=require('express');

//引入模块
var admin =require('./routes/admin.js');

var index =require('./routes/index.js')

var app=new express();  /*实例化*/

//admin
//admin/user

app.use('/',index);

/*
/admin/login

 /admin/user

* */

app.use('/admin',admin);

app.listen(3004,'127.0.0.1');
```