# Electron-vue 本地数据库操作

## electron 应用操作数据库的几种方式

* 远程 api 接口
* 连接远程数据库（安全起见，不建议使用）
* 连接本地数据库（`nedb`、`sqlite`），`localStorage` 存储空间有限（5M）

`nedb` 数据库文档

https://github.com/louischatriot/nedb，nedb数据库和mongodb数据库的操作方法几乎一模一样。

## electron-vue 中使用 nedb 数据库

https://simulatedgreg.gitbooks.io/electron-vue/content/cn/savingreading-local-files.html  

* 安装 `nedb` 数据

```
cnpm install nedb --save
```

* 新建 `src/renderer/datastore.js`

```js
import Datastore from 'nedb'
		import path from 'path'
		import { remote } from 'electron'

		export default new Datastore({
  			autoload: true,
  			filename: path.join(remote.app.getPath('userData'), '/data.db')
		})
```

* `src/renderer/main.js`

```js
import db from './datastore.js'

/* ... */
Vue.prototype.$db = db
```

* 在 vue 的组件里面实现数据的增加、修改、删除、显示

```js
this.$db.insert({},function(){

})

this.$db.find({},function(){

})

this.$db.update({条件},{$set:{更改的数据}},function(){

})

this.$db.remove({条件},{},function(){

})
```

https://www.w3cschool.cn/electronmanual/electronmanual-electronapp.html