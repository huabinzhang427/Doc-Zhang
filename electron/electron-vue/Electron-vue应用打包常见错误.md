# electron-vue  应用打包常见错误解决方案

## 创建应用托盘的时候可能会遇到错误

把托盘图片放在根目录 `static` 里面，然后注意使用下面写法

```js
var tray = new Tray(path.join(__static,'favicon.ico'));
```

如果托盘路径没有问题，还是包托盘相关错误的话，把托盘对应的图片换成 `.png` 格式重试。

## 模块问题可能会遇到的错误

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190312-153801@2x.png)

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190312-153828@2x.png)



解决方法：

* 删掉 `node_modules` 然后重新用 `npm install` 安装依赖
* 如果上面方法还是解决不了。可以通过安装 `yarn` 用 `yarn` 来安装模块（`yran` 替代 `npm install /cnpm install`）
* 如果上面两个方法都不可行，用手机创建一个热点电脑连上热点。重试上面任 意一种方法

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/electron/images/WX20190312-153929@2x.png)

注意，在 `windows` 系统下运行 `npm run build` 会自动生成 `.exe` 应用文件。在 `mac` 下运行 `npm run build` 会自动生成 `.dmg` 应用文件。所以最好在各自的系统运行生成各自的应用，避免一些未知的错误。

