# Electron基础概念

## Electron 运行流程

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-152328@2x.png)

运行 electron 项目过程中，首先进入配置文件 `package.json`，找到项目入口文件

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-152619@2x.png)

调用主进程，进入主进程。主进程准备好后，创建 `BrowserWindow`，然后通过 `BrowserWindow` 来加载渲染进程 `index.html`。

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-153016@2x.png)

## Electron 主进程与渲染进程

Electron 运行 `package.json` 的 main 脚本的进程被称为**主进程**。 在主进程中运行的脚本通过创建 web 页面来展示用户界面。 一个 Electron 应用总是有且只有一个主进程。

由于 Electron 使用了 Chromium(谷歌浏览器)来展示 web 页面，所以 Chromium 的多进程架构也被使用到。 **每个 Electron 中的 web 页面运行在它自己的渲染进程中**。

主进程使用 BrowserWindow 实例创建页面。每个 BrowserWindow 实例都在自己的渲染进程里运行页面。**当一个 BrowserWindow 实例被销毁后，相应的渲染进程也会被终止**。

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-154127@2x.png)


* **进程**：系统进行资源分配和调度的基本单位，是操作系统结构的基础。

* **线程**：一个程序里的一个执行路线，*一个进程内部的控制序列*。

一个程序至少有一个进程,一个进程至少有一个线程。

## Electron 渲染进程中使用 Node.js

在普通的浏览器中，web 页面通常在一个沙盒环境中运行，不被允许去接触原生的资源。 然而 Electron 的用户**在 Node.js 的 API 支持下可以在页面中和操作系统进行一些底层交互**。

**Nodejs 在主进程和渲染进程中都可以使用**。渲染进程因为安全限制，不能直接操作原 生 GUI。虽然如此，因为集成了 Nodejs，渲染进程也有了操作系统底层 API 的能力，Nodejs 中常用的 `path`、`fs`、`crypto` 等模块在 Electron 可以直接使用，方便我们处理链接、路径、 文件 MD5 等，同时 npm 还有成千上万的模块供我们选择。

**读取本地文件**

```js
var fs = require('fs');
var content = document.getElementById('content'); 
var button = document.getElementById('button'); button.addEventListener('click',function(e){
        fs.readFile('package.json','utf8',function(err,data){
        content.textContent = data;
        console.log(data);
        }); 
});
```


