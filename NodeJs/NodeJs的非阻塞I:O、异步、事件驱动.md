# NodeJs 非阻塞 I/O、异步、事件驱动

## 概述

在 `Java`、`PHP` 或者 `.net` 等服务器端语言中，会为每一个客户端连接创建一个新的线程。 而每个线程需要耗费大约 2MB 内存。也就是说，理论上，一个 8GB 内存的服务器可以同时连接的最大用户数为 4000 个左右。要让 Web 应用程序支持更多的用户，就需要增加服务器的数量，而 Web 应用程序的硬件成本当然就上升了。

`Node.js` 不为每个客户连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了，就触发一个内部事件，通过**非阻塞 I/O**、**事件驱动**机制，让 `Node.js` 程序宏观上也是并行的。 使用 `Node.js`，一个 8GB 内存的服务器，可以同时处理超过 4 万用户的连接。

## 回调处理异步

```js
// 错误写法，注意异步执行！！
// function getData() {
//     var result = ''
//     setTimeout(() => {
//        result = '这时请求到的数据' 
//     }, 200);
//     return result
// }

// console.log(getData())

// 通过回调函数处理异步请求
function getData(callback) {
    var result = ''
    setTimeout(() => {
        result = '这是请求到的数据'
        callback(result)
    }, 200);
}

getData(function(data) {
    console.log(data)
})
```

```
$ node testIo.js

$ node testIo.js
这是请求到的数据
```

## `events` 模块事件驱动处理异步

`Node.js` 有多个内置的事件，我们可以通过引入 `events` 模块，并通过实例化 `EventEmitter` 类来绑定和监听事件。

```js
var events = require('events')

var eventEmitter = new events.EventEmitter()

// 定义广播接收器
eventEmitter.on('tome', function() {
    console.log('接收到广播事件了～')
})

setTimeout(() => {
    console.log('到点儿了发送广播啦！')
    eventEmitter.emit('tome') // 发送广播
}, 1000);
```

```
$ node testIo.js
到点儿了发送广播啦！
接收到广播事件了～
```

