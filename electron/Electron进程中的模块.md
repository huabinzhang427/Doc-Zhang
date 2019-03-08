# Electron进程中的模块

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-161303@2x.png)

## Electron remote 模块

Electron 中, 与 GUI 相关的模块(如 `dialog`, `menu` 等)只存在于主进程，而不在渲染进程中。 为了能从渲染进程中使用它们，需要用 ipc 模块来给主进程发送进程间消息。使用 `remote` 模块，可以调用主进程对象的方法，而无需显式地发送进程间消息，这类似于 Java 的 RMI。

```js
var BrowserWindow = require('electron').remote.BrowserWindow
```

## Electron menu 模块

Electron 中 menu 模块可以用来创建原生菜单，它可用作应用菜单和 context 菜单。这个模块是一个主进程的模块，并且可以通过 remote 模块给渲染进程调用。

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190308-171745@2x.png)


## Electron ipcMain/ipcRenderer 模块

有时候我们想在渲染进程中通过一个事件去执行主进程里面的方法。或者在渲染进程中通知 主进程处理事件，主进程处理完成后广播一个事件让渲染进程去处理一些事情。这个时候就用到了主进程和渲染进程之间的相互通信。

* **ipcMain**：当在主进程中使用时，它处理从渲染器进程(网页)发送出来的异步和同步信息,
当然也有可能从主进程向渲染进程发送消息。
* **ipcRenderer**：使用它提供的一些方法从渲染进程 (web 页面) 发送同步或异步的消息到主
进程。 也可以接收主进程回复的消息。


### 场景 1:渲染进程给主进程发送异步消息:

```js
//渲染进程
const { ipcRenderer } = require('electron')
ipcRenderer.send('msg',{name:'张三'}); //异步
```

```js
//主进程
const { ipcMain } = require('electron');
ipcMain.on(''msg'',(event,arg) => { })
```

### 场景 2:渲染进程给主进程发送异步消息，主进程接收到异步消息以后通知渲染进程:

```js
//渲染进程
const { ipcRenderer } = require('electron')
ipcRenderer.send('msg',{name:'张三'}); //异步
```

```js
//主进程
const { ipcMain } = require('electron');
ipcMain.on(''msg'',(event,arg) => { event.sender.send('reply', 'pong');
})
```

```js
//渲染进程
const { ipcRenderer } = require('electron')
ipcRenderer.on('reply', function(event, arg) { console.log(arg); // prints "pong"
});
```

### 场景 3:渲染进程给主进程发送同步消息:

```js
//渲染进程
const { ipcRenderer } = require('electron')
const msg = ipcRenderer.sendSync('msg-a'); console.log(msg)
```

```js
//主进程 
ipcMain.on('msg-a',(event)=> {
    event.returnValue = 'hello';
})
```

## Electron 渲染进程与渲染进程之间的通信

### Electron 渲染进程通过 `localstorage` 给另一个渲染进程传 值

```js
localStorage.setItem(key,value);
localStorage.getItem(key);
```

### 通过 BrowserWindow 和 webContents 模块实现渲染进程和渲染进程的通信

`webContents` 是一个事件发出者.它负责渲染并控制网页，也是 BrowserWindow 对象的属性。

在这里主进程作为中间媒介，主进程先获取渲染进程传过来的值，再把值通过 `webContents` 发送给另一个渲染进程。

* 获取当前窗口的 `id`

```js
const winId = BrowserWindow.getFocusedWindow().id;
```

* 监听当前窗口加载完成的事件

```js
win.webContents.on('did-finish-load',(event) => {
})
```

* 同一窗口之间广播数据

```js
win.webContents.on('did-finish-load',(event) => {
win.webContents.send('msg',winId,'我是 index.html 的数据'); })
```

* 通过 `id` 查找窗口

```js
let win = BrowserWindow.fromId(winId);
```

## Electron shell 模块

在用户默认浏览器中打开 URL 的示 例

```js
var shell = require('shell');
shell.openExternal('https://github.com');
```

## Electron dialog 模块

`dialog` 模块提供了 api 来展示原生的系统对话框，例如打开文件框，alert 框， 所以 web 应用可以给用户带来跟系统应用相同的体验。

```js
// 
dialog.showErrorBox('title','content');
// 
dialog.showMessageBox({ type:'info',
title: 'message', message: 'hello', buttons:['ok','cancel']
},(index) => {
if ( index == 0 ) {
console.log('You click ok.'); } else {
console.log('You click cancel'); }
})
// 
dialog.showOpenDialog({ properties:['openFile','openDirectory']
}, (files) => { console.log(files);
})
// 
dialog.showSaveDialog({ title: 'save some file', filters: [
{ name: 'Images', extensions: ['jpg', 'png', 'gif'] },
{ name: 'Movies', extensions: ['mkv', 'avi', 'mp4'] }, { name: 'Custom File Type', extensions: ['as'] },
{ name: 'All Files', extensions: ['*'] }
] },(filename) => {
console.log(filename); })
```

更多模块详情

https://electronjs.org/docs

