# NodeJs 概述与常用模块

https://nodejs.org/zh-cn/

## NodeJs介绍

`Node.js` 是一个 JS 运行环境(runtime)。它让 JS 可以开发后端程序，实现几乎其他后端语言实现的所有能力。

`Node.js` 是基于 V8 引擎，V8 是 Google 发布的开源 JS 引擎，本身就是用于 Chrome 浏览器的 JS 解释部分。**Ryan Dahl** 鬼才般的把 V8 搬到了服务器上，用于做服务器的软件。

* NodeJs 语法完全是 js 语法，只要懂 js 基础就可以学会 NodeJs 后端开发。（它打破了 js 只能在浏览器中运行的局面，前后端编程环境统一，大大降低开发成本）
* NodeJs 超强的高并发能力。

在 java、php 或者 .net 等服务器端语言中，会为每个客户端连接创建一个新的线程。而每个线程需要耗费大约 2MB 内存。也就是说，理论上一个 8G 内存的服务器可以同时连接的最大用户数为 4000 个左右。要让 web 应用程序支持更多的用户，就需要增加服务器的数量，而 web 应用程序的硬件成本就上升了。

NodeJs 不为每个客户端连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了，就触发一个内部事件，通过非阻塞 I/O、事件驱动机制，让 NodeJS 程序宏观上也是并行的。使用 Nodejs，一个 8G 内存的服务器，可以同时处理超过 4 万用户的连接。

* 在 V8 js 引擎内部使用一种全新的编译技术。这意味着开发者编写的高端 js 脚本代码与开发者编写的低端的 C语言具有非常相近的执行效率。

`Node.js 自身哲学是---花最小的硬件成本，追求更高的并发，更高的处理能力。`

Node.js 可以做什么？

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/NodeJs/images/dowhat.png)

## NodeJS 常用模块

如果我们使用 PHP 来编写后端的代码时，需要 Apache 或者 Nginx 的 HTTP 服务器，来处理客户端的请求相应。不过对 Node.js 来说，概念完全不一样了。**使用 Node.js 时， 我们不仅仅在实现一个应用，同时还实现了整个 HTTP 服务器**。

### 1.HTTP、URL 模块

```js
var http = require('http') // 引入 http 模块
var url = require('url') // 引入 url 模块

http.createServer(function(req, res) { // 创建服务器，通过 request, response 参数来接收和响应数据
    if(req.url != '/favicon.ico') {
        console.log('服务器收到了请求:'+req.url) // /news?age=12&name=yao
        console.log(url.parse(req.url, true))
        var result = url.parse(req.url, true).query
        console.log(result)
        console.log(result.name)
        // console.log('get请求的数据='+url.parse(req.url, true))
    }
    // 设置 HTTP 头部，状态码是 200，文件类型是 html，字符集是 utf-8
    res.writeHead(200, {'Content-Type':'text/html;charset=UTF-8'})
    // 发送响应数据，End 方法使 Web 服务器停止处理脚本并返回当前结果
    res.end('恭喜百洛普李杰荣登福布斯排行榜第一位！')
}).listen(8888) // 使用 listen 方法绑定端口
// 终端打印信息
console.log('Server running at http://localhost:8888')
```

浏览器输入网址：`http://localhost:8888/news?age=12&name=yao`

```
服务器收到了请求:/news?age=12&name=yao
Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '?age=12&name=yao',
  query: [Object: null prototype] { age: '12', name: 'yao' },
  pathname: '/news',
  path: '/news?age=12&name=yao',
  href: '/news?age=12&name=yao' }
[Object: null prototype] { age: '12', name: 'yao' }
yao
```

注意，我们本地写一个 js，打死都不能直接拖入浏览器运行，但是有了 node，我 们任何一个 js 文件，都可以通过 node 来运行。也就是说，**node 就是一个 js 的执行环境**。

`http.createServer()`创建以恶搞服务器时，回调函数接收到的请求 `req.url` 属性，表示用户的请求 URl 地址。**所有的路由设计，都是通过 `req.url` 来实现的**。我们比较关心的不是拿到 URL，而是**识别这个 URL**。

识别 URL，用到了下面的 url 模块

* `url.parse()`：解析 URL（重点掌握这个！！！）
* `url.format(urlObject)`：上面操作的逆向操作
* `url.resolve(from, to)`：添加或者替换地址

**Nodejs 自启动工具 supervisor**

supervisor 会不停的 watch 你应用下面的所有文件，发现有文件被修改，就重新载入程序文件这样就实现了部署，修改了程序文件后马上就能看到变更后的结果。

通过 `npm install -g supervisor` 进行安装，在项目目录中，使用 supervisor 代替 node 命令启动应用 `supervisor xxx.js`。

