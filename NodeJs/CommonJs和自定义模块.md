# CommonJs 和 自定义模块

## CommonJs

JavaScript 是一个强大面向对象语言，它有很多快速高效的解释器。然而， JavaScript 标准定义的 API 是为了构建基于浏览器的应用程序。并没有制定一个用于更广泛的应用程序的标准库。,而不只是停留在小脚本程序的阶段。用 `CommonJS API` 编写出的应用，不仅可以利用 JavaScript 开发客户端应用，而且还可以编写以下应用 。

* 服务器端 JavaScript 应用程序。(nodejs)
* 命令行工具
* 桌面图形界面应用程序

**CommonJS 就是模块化的标准，nodejs 就是 CommonJS(模块化)的实现。**

## Nodejs 中的模块化

Node 应用由模块组成，采用 CommonJS 模块规范。

在 Node 中，模块分为两类:

* 一类是 Node 提供的模块,称为**核心模块**
* 另一类是用户编写的模块，称为**文件模块**

`核心模块`部分在 Node 源代码的编译过程中，编译进了二进制执行文件。在 Node 进程启动时，部分核心模块就被直接加载进内存中，所以这部分核心模块引入时，文件定位和编译执行这两个步骤可以省略掉，并且在路径分析中优先判断，所以它的加载速度是最快的。 如:HTTP 模块 、URL 模块、Fs 模块都是 nodejs 内置的**核心模块，可以直接引入使用**。

`文件模块`则是在运行时动态加载，需要完整的路径分析、文件定位、编译执行过程、 速度相比核心模块稍微慢一些，但是用的非常多。这些模块需要我们自己定义。

CommonJS(Nodejs)中自定义模块的规定:

1. 我们可以把公共的功能抽离成为一个单独的 js 文件作为一个模块，默认情况下面这 个模块里面的方法或者属性，外面是没法访问的。如果要让外部可以访问模块里面的方法或 者属性，就必须在模块里面通过 `exports` 或者 `module.exports` 暴露属性或者方法。
2. 在需要使用这些模块的文件中，通过 `require` 的方式引入这个模块。这个时候就可以 使用模块里面暴露的属性和方法。

```js
var tools = require('tools') 
// tools 默认目录下没有，会在 node_modules 里面找这个模块，
// 如果想实现核心模块一样直接引入自定义模块，
// 我们可以通过 `npm init --yes` 在自定义模块目录下生成一个 `package.json`，里面有入口文件设置直接指向我们定义的模块

// tools.sayHello()
console.log(tools.sayHello())
```
## NPM 介绍

npm 是**世界上最大的开放源代码的生态系统**。我们可以通过 npm 下载各种各样的包，这些源代码(包)我们可以在 https://www.npmjs.com 找到。

npm 是随同 NodeJS 一起安装的包管理工具，能解决 NodeJS 代码部署上的很多问题，常见的使用场景有 以下几种:

* 允许用户从 NPM 服务器下载别人编写的第三方包到本地使用
* 允许用户从 NPM 服务器下载并安装别人编写的命令行程序(工具)到本地使用。
* 允许用户将自己编写的包或命令行程序上传到 NPM 服务器供别人使用

### NPM 命令详解

* `npm -v` 查看 npm 版本
* `npm install moudleName` 安装模块
* `npm uninstall moudleName` 卸载模块
* `npm list` 查看当前目录下已安装的 node 包
* `npm info moudleName` 查看模块的版本
* `npm install moudleName@1.8.0` 指定版本安装

## package.json

`package.json` 定义了这个项目所需要的各种模块,以及项目的配置信息(比如名称、版本、许可证等元数据)

* 创建 `package.json`

```
npm init
npm init –yes
```

* `package.json` 文件

```json
{
"name": "test", "version": "1.0.0", "description": "test",
"main": "main.js", "keywords": [
"test" ],
"author": "wade",
"license": "MIT", "dependencies": {
"express": "^4.10.1" },
"
"jslint": "^0.6.5" }
}
```

* 安装模块并把模块写入 package.json(依赖)

```
npm install babel-cli --save-dev（写入到 devDependencies）
npm install 模块 --save（写入到 dependencies）
 
npm install 模块 --save-dev
```

`dependencie`， 配置当前程序所依赖的其他包。

`devDependencie`， 配置当前程序所依赖的其他包，只会下载模块，而不下载这些模块的测试和文档框架

```json
"dependencies": {
"ejs": "^2.3.4", 
"express": "^4.13.3", 
"formidable": "^1.0.17"
}
```
`^` 表示第一位版本号不变，后面两位取最新的

`~` 表示前两位不变，最后一个取最新

`*` 表示全部取最新

## 安装淘宝镜像

http://www.npmjs.org npm 包官网

https://npm.taobao.org/ 淘宝 npm 镜像官网

淘宝 NPM 镜像是一个完整 `npmjs.org` 镜像，你可以用此代替官方版本(只读)，同步频 率目前为 10 分钟一次以保证尽量与官方服务同步。

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

