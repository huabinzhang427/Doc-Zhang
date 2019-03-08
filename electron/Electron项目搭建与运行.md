# Electron 项目搭建与使用

https://electronjs.org/docs/tutorial/installation

## 安装 electron

全局安装

```
npm install electron -g
```

## 项目搭建

### 手动搭建

适合初次学习，主要是理解项目结构，功能。具体步骤如下

* 新建一个项目目录
* 在该目录下新建文件 `index.html`、`main.js`
* 打开终端，在该目录下运行 `npm init`，按照步骤，最后生成 `package.json` 文件

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-150441@2x.png)

* 在 `main.js` 中的代码如下

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-150933@2x.png)

* 运行

```
electron . 
```

### 通过脚手架搭建

一些脚手架工具可以帮助我们方便快速的创建、运行和打包 electron 项目， 比如通过 `electron-forge` 搭建 electron 项目

```
npm install -g electron-forge

electron-forge init my-new-app

cd my-new-app

npm start
```


