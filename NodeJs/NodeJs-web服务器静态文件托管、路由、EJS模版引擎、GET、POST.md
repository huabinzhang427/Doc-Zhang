# NodeJs web服务器的静态文件托管、路由、EJS模版引擎、GET、POST

## 静态文件托管

静态 web 服务器的封装，对 `http.createServer()` 中的内容进行进一步封装操作，也就是 NodeJs 中的**模块化的思想**。

示例（改造之前的代码）：

创建 `module/router.js`，将之前在 `http.createServer()` 中的内容放到路由里，再稍作修改

```js
// router.js
var fs = require('fs')
var url = require('url')
var path = require('path')

// 私有方法
function getMime(extname) { // 通过后缀名获取文档类型 mime type的方法
    // 把读取数据改成同步方法
    var data = fs.readFileSync('./mime.json')
    // data.toString() 转换为 json 字符串
    // 把 json 字符串转换为 json 对象
    var mimes = JSON.parse(data.toString())
    return mimes[extname] || 'text/html'
}

exports.statics = function(req, res, staticPath) { // 静态资源目录
    // 获取 pathname
    var pathName = url.parse(req.url).pathname
    if(pathName == '/') {
        pathName = '/index.html' // 设置默认 pathname => /index.html
    }
    var extName = path.extname(pathName) // 获取 path 后缀名

    console.log('pathName='+pathName)
    console.log('extName='+extName)
    
    if(pathName != '/favicon.ico') { // 过滤掉 favicon.ico 请求
        // 文件操作获取 static 下的文件
        fs.readFile(staticPath+'/'+pathName, function(err, data) {
            if(err) { // 没有该文件
                console.log('没有该文件')

                fs.readFile(staticPath+'/404.html', function(err, data404) {
                    if(err) {
                        console.log(err)
                    }
                    res.writeHead(404, {"Content-Type":"text/html;charset='utf-8'"})
                    res.write(data404)
                    res.end() // 结束响应
                })
            } else { // 返回该文件
                console.log('返回该文件')
                var mime = getMime(extName)
                res.writeHead(200, {"Content-Type":''+mime+";charset='utf-8'"})
                res.write(data)
                res.end() // 结束响应
            }
        })
    }
}
```

然后回到创建服务的代码中

```js
var http = require('http')

var router = require('./model/router.js')

http.createServer(function(req, res) {
    router.statics(req, res, './static')
}).listen(8899)
```
这样就使得代码看上去更加清晰。就是通过模块化的思想来封装逻辑代码，再在外边引入模块即可。

## 路由


**官方说明：**

> 路由(Routing)是由一个 URI(或者叫路径)和一个特定的 HTTP 方法(`GET`、`POST` 等)组成 的，涉及到应用如何响应客户端对某个网站节点的访问。

简单说就是，**路由指的就是针对不同请求的 URL，处理不同的业务逻辑**。

<!-- ![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/NodeJs/images/WX20190315-141802@2x.png) -->

```js
// ...
http.createServer(function(req, res) {
    // 路由，就是针对不同请求的 URL，处理不同的业务逻辑
    var pathName = url.parse(req.url).pathname
    if(pathName == '/login') {
        res.end('login')
    } else if(pathName == '/register') {
        res.end('register')
    } else {
        res.end('index')
    }
}).listen(8899)
```

## EJS 模版引擎

`EJS` **后台引擎**，可以把我们数据库和文件读取的数据显示到 html 页面上面。它是一个第三方模块。

https://www.npmjs.com/package/ejs

安装

```
cnpm install ejs --save
```

之前我们加载的都是静态页面，接下来我们根据不同的路由地址来进行不同的操作（比如把后台数据库的数据渲染到模版上面）。这是之前静态页面所无法完成的，因此我们就用到了 EJS 模版引擎来进行数据渲染。

NodeJs 中使用：
```js
ejs.renderFile(filename, data, options, function(err, str){
    // str => Rendered HTML string
});
```

ESJ 常用标签

* `<% %>`，流程控制标签
* `<%= %>`，输出标签（原文输出）
* `<%- %>`，输出标签（HTML 会被浏览器解析）

```html
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>

 
<a href="<%= url %>"><img src="<%= imageURL %>" alt=""></a><ul>

<ul>
<% for(var i = 0 ; i < news.length ; i++){ %>
    <li><%= news[i] %></li> <% } %>
</ul>
```

## GET、POST

超文本传输协议(HTTP)的设计目的是保证客户端机器与服务器之间的通信。

在客户端和服务器之间进行请求-响应时，两种最常被用到的方法是: **GET** 和 **POST**。

* **GET**，从指定的资源请求数据。(一般用于获取数据)
* **POST**，向指定的资源提交要被处理的数据。(一般用于提交数据)

**获取 GET 传值：**

```js
var urlinfo=url.parse(req.url,true); urlinfo.query();
```

**获取 POST 传值：**

```js
var postData = ''; // 数据块接收中
req.on('data', function (postDataChunk) { postData += postDataChunk;
});
// 数据接收完毕，执行回调函数
req.on('end', function () { try {
postData = JSON.parse(postData); } catch (e) { }
req.query = postData;
console .log(q uerystring .parse(postData));
});
```

## 路由封装

模块化的路由封装

代码示例：

```js
// model.js
var ejs = require('ejs')
var fs = require('fs')

var app = {
    login: function(req, res) {
        ejs.renderFile('./views/form.ejs', {}, function(err, data) {
            res.end(data)
        })
    },
    dologin: function(req, res) {
        var method = req.method.toLowerCase()
        if(method == 'get') {
            console.log(url.parse(req.url, true).query)
            res.end('dologin get')
        } else if(method == 'post') {
            var postData = ''
            req.on('data', function(chunk) {
                postData += chunk
            })
            req.on('end', function() {
                console.log(postData)

                fs.appendFile('login.txt', postData + '\n', function(err) {
                    if(err) {
                        console.log(err)
                        return
                    }
                    console.log('写入数据成功！')
                })
                res.end("<script>alert('haha');history.back();</script>")
            })
        }
    },
    default: function(req, res) {
        ejs.renderFile('./views/error.ejs', {}, function(err, data) {
            res.end(data)
        })
    }
}
module.exports = app
```

```js
var model = require('./model/model.js')

http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/html;charset="utf-8"'})

    var pathName = url.parse(req.url).pathname.replace('/', '')
    if(pathName != 'favicon.ico') {
        try {
            model[pathName](req, res)
        } catch (error) {
            model['default'](req, res)
        }
        res.end(pathName)
    }
}).listen(9990)
```


