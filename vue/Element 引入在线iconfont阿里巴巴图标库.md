# Element 引入在线 iconfont(阿里巴巴图标库)

* 登录 [Iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/home/index?spm=a313x.7781069.1998910419.2)

* 我的项目 >> 创建项目

注意其中 `FontClass/Symbol 前缀`的内容不要和项目中的引入的 ui 库起冲突。

* 在图标库中选择图标，添加入库

* 进入库中，选择需要的图标，添加到自己创建的项目中。

* 进入我的项目，在刚创建的项目下选择 `Font class` 生成 icon 在线 css 链接。

---
* 在 vue 项目中创建 `./assets/css/iconfont.css` 文件(文件名自定义)

```css
[class^="el-icon-ali"], [class*="el-icon-ali"]
{
  font-family:"iconfont" !important;
  font-size:16px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```
这里的 `el-icon-ali` 是刚才创建图标项目时 `FontClass/Symbol 前缀`的内容。

* 在 `index.html` 引入刚才图库项目在线生成的 css 链接

```html
<!-- ... -->
<head>
    <link rel='stylesheet' href='在线 css 链接'>
</head>
<!-- ... -->
```
* 在 `App.vue` 中引入自己创建的 css 文件

```html
<style src='./assets/css/iconfont.css'></style>
```

* 在项目中使用 icon

```html
<i class='iconfont el-icon-ali.....'></i>
<el-button icon='iconfont el-icon-ali.....'></el-button>
```

注意，icon 引入前必有加上 `iconfont`。

