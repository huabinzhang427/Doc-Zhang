# NideJs Express框架-路由封装 

http://www.runoob.com/nodejs/nodejs-express-framework.html

Express 框架核心特性：

* 可以设置**中间件**来响应 HTTP 请求
* 定义了**路由表**用于执行不同的 HTTP 请求动作
* 可以通过向模板传递参数来动态渲染 HTML 页面

## 安装依赖

```
cnpm install express --save
```

以下几个重要的模块是需要与 express 框架一起安装的：

* **body-parser**，node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据
* **cookie-parser**，一个解析 Cookie 的工具，通过 req.cookies 可以取到传过来的 cookie，并把它们转成对象
* **multer**，node.js 中间件，用于处理 `enctype="multipart/form-data"` （设置表单的 MIME 编码）的表单数据

```
cnpm install body-parser --save
cnpm install cookie-parser --save
cnpm install multer --save
```

## 请求和响应

Express 应用使用回调函数的参数： request 和 response 对象来处理请求和响应的数据。

```js
app.get('/', function (req, res) {
   // --
})
```

**Request 对象**，表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性。

**Response 对象** ，表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据。

## 路由

路由决定了由谁(指定脚本)去响应客户端请求。在 HTTP 请求中，我们可以通过路由提取出请求的 URL 以及 GET/POST 参数。

```js
var express = require('express');
var app = express();
 
//  主页输出 "Hello World"
app.get('/', function (req, res) {
   console.log("主页 GET 请求");
   res.send('Hello GET');
})
 
 
//  POST 请求
app.post('/', function (req, res) {
   console.log("主页 POST 请求");
   res.send('Hello POST');
})
```

## 静态文件

Express 提供了内置的中间件 **express.static** 来设置静态文件如：图片， CSS, JavaScript 等。

```js
app.use(express.static('public'));
// public/images/logo.png
// 访问 http://localhost:port/images/logo.png
```

## GET 方法

比如在表单中通过 GET 方法提交两个参数，我们可以使用 `server.js` 文件内的 **process_get** 路由器来处理输入：

```html
<html>
<body>
<form action="http://127.0.0.1:8081/process_get" method="GET">
First Name: <input type="text" name="first_name">  <br>
 
Last Name: <input type="text" name="last_name">
<input type="submit" value="Submit">
</form>
</body>
</html>
```

```js
// server.js
var express = require('express');
var app = express();
 
app.use(express.static('public'));
 
app.get('/index.htm', function (req, res) {
   res.sendFile( __dirname + "/" + "index.htm" );
})
// process_get 路由
app.get('/process_get', function (req, res) {
 
   // 输出 JSON 格式
   var response = {
       "first_name":req.query.first_name,
       "last_name":req.query.last_name
   };
   console.log(response);
   res.end(JSON.stringify(response));
})
 
var server = app.listen(8081, function () {
})
```


## POST 方法

比如在表单中通过 POST 方法提交两个参数，我们可以使用 `server.js` 文件内的 **process_post** 路由器来处理输入：

```html
<!-- index.htm -->
<html>
<body>
<form action="http://localhost:port/process_post" method="POST">
First Name: <input type="text" name="first_name">  <br>
 
Last Name: <input type="text" name="last_name">
<input type="submit" value="Submit">
</form>
</body>
</html>
```

```js
// server.js
var express = require('express');
var app = express();
var bodyParser = require('body-parser');
 
// 创建 application/x-www-form-urlencoded 编码解析
var urlencodedParser = bodyParser.urlencoded({ extended: false })
 
app.use(express.static('public'));
 
app.get('/index', function (req, res) {
   res.sendFile( __dirname + "/" + "index.htm" );
})
 
app.post('/process_post', urlencodedParser, function (req, res) {
 
   // 输出 JSON 格式
   var response = {
       "first_name":req.body.first_name,
       "last_name":req.body.last_name
   };
   console.log(response);
   res.end(JSON.stringify(response));
})
 
var server = app.listen(8081, function () {
 
})
```

## 文件上传

比如创建一个用于上传文件的表单，使用 POST 方法，表单 `enctype` 属性设置为 `multipart/form-data`：

```html
<!-- index.html -->
<html>
<head>
<title>文件上传表单</title>
</head>
<body>
<h3>文件上传：</h3>
选择一个文件上传: <br />
<form action="/file_upload" method="post" enctype="multipart/form-data">
<input type="file" name="image" size="50" />
<br />
<input type="submit" value="上传文件" />
</form>
</body>
</html>
```

```js
// server.js
var express = require('express');
var app = express();
var fs = require("fs");
 
var bodyParser = require('body-parser');
var multer  = require('multer');
 
app.use(express.static('public'));
app.use(bodyParser.urlencoded({ extended: false }));
app.use(multer({ dest: '/tmp/'}).array('image'));
 
app.get('/index', function (req, res) {
   res.sendFile( __dirname + "/" + "index.html" );
})
 
app.post('/file_upload', function (req, res) {
 
   console.log(req.files[0]);  // 上传的文件信息
 
   var des_file = __dirname + "/" + req.files[0].originalname;
   fs.readFile( req.files[0].path, function (err, data) {
        fs.writeFile(des_file, data, function (err) {
         if( err ){
              console.log( err );
         }else{
               response = {
                   message:'File uploaded successfully', 
                   filename:req.files[0].originalname
              };
          }
          console.log( response );
          res.end( JSON.stringify( response ) );
       });
   });
})
 
var server = app.listen(8081, function () {
 
})
```