# Cookie 的使用

## 简介

* cookie 是存储于访问者的计算机的变量。可以让我们用**同一个浏览器访问同一个域名的时候共享数据**。
* http 是无状态协议。简单的说，当你浏览一个页面，然后转到同一个网站的另一个页面，服务器无法识别到这是同一个浏览器在访问同一个网站。每一次的访问，都是没有任何关系的。
* cookie：当访问一个页面的时候，服务器在下行 http 报文中，命令浏览器存储一个字符串；浏览器再访问**同一个域**的时候，将把这个字符串携带到上行 http 请求中。第一次访问一个服务器，不可能携带 cookie。必须是服务器得到这次请求，在下行响应报文中，携带 cookie 信息，此后每一次浏览器往这个服务器发出的请求，都会携带这个 cookie。

## cookie 特点

* cookie 保存在浏览器本地
* 正常设置的 cookie 是不加密的，用户可以自由看到
* 用户可以删除 cookie，或者禁用它
* cookie 可以被篡改
* cookie 可以用于攻击
* cookie 存储量很小，被 localStorage 替代

## Cookie 的使用

安装
```
cnpm instlal cookie-parser --save
```

```js
// 引入
var cookieParser = require('cookie-parser')
```

```js
// 设置中间件
app.use(cookieParser())
```

```js
// 设置 cookie
res.cookie("name",'zhangsan',{maxAge: 900000, httpOnly: true});
// HttpOnly 默认 false 不允许 客户端脚本访问
```

```js
// 获取 cookie
req.cookies.name
// 删除 cookie
res.cookie('rememberme', '', { expires: new Date(0)});
res.cookie('username','zhangsan',{domain:'.ccc.com',maxAge:0,httpOnly:true});
```

## Cookie 配置属性

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/NodeJs/images/WX20190403-150320@2x.png)

**属性说明**

* `domain`: 域名
* `name=value`: 键值对，可以设置要保存的 key/value，注意这里的 name 不能和其它属性项的名字一样
* `expires`: 过期时间（秒），在设置的某个时间点后该 cookie 就会失效
* `maxAge`: 最大失效时间（毫秒）
* `secure`: 当 secure 值为 true 时， cookie 在 http 中是无效，在 https 中才有效
* `path`: 表示 cookie 影响到的路，如 path=/，如果路径不能匹配时，浏览器则不发送这个 cookie
* `httpOnly`: 是微软对 cookie 做的扩展，如果在 cookie 设置了该属性，则通过程序(js脚本、applet等)将无法读取到 cookie 信息，防止 xss 攻击产生
* `signed`: 表示是否签名 cookie，设为 true 会对这个 cookie 签名，这样就需要用 res.signedCookies 而不是 res.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie 值会重置为它的初始值

## 加密 Cookie

配置中间件的时候需要传参

```js
var cookieParser = require('cookie-parser');
app.use(cookieParser('123456'))
```

设置 cookie 的时候配置 signed 属性

```js
res.c ookie('userinfo','hahaha',{domain:'.c cc.c om',maxAge :900000,httpOnly :true,signed :true})
```

 signedCookies 调用设置的 cookie

 ```js
console.log(req.signedCookies)
 ```