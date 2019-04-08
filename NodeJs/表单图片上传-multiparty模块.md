# 表单图片上传-multiparty模块

## 使用

https://www.npmjs.com/package/multiparty


安装

```
npm install multiparty
```

引入

```js
var multiparty = require('multiparty') // 既可以获取 form 表单的数据，也可以实现上传图片
```

上传图片的地方实例化

```js
var form = new multiparty.Form()
```
上传图片保存的目录

```js
form.uploadDir('目录')
```

获取提交的数据以及图片上传成功返回的图片信息

```js
form.parse(req, function(err, fields, files) {
    console.log(fields) // 获取表单的数据
    console.log(files) // 图片上传成功返回的信息
}
```

页面 form 表单要加入 `enctype="multipart/form-data"`

```html
<form action="/doProductAdd" method="post" enctype="multipart/form-data">
    <ul>
        <li>  商品名称: <input type="text" name="title"/></li>
        <li>  商品图片: <input type="file" name="pic"/></li>
        <li>  商品价格: <input type="text" name="price"/></li>
        <li>  商品邮费: <input type="text" name="fee"/></li>
        <li>商品描述:
            <textarea name="description" id="" cols="60" rows="8"></textarea>
        </li>
        <li>
        <br/>
            <button type="submit" class="btn btn-default">提交</button>
        </li>
    </ul>
</form>
```

```js
app.post('/doProductAdd',function(req,res){

    //获取表单的数据 以及post过来的图片
    var form = new multiparty.Form();

    form.uploadDir='upload'   //上传图片保存的地址     目录必须存在

    form.parse(req, function(err, fields, files) {

        //获取提交的数据以及图片上传成功返回的图片信息
        //
        //console.log(fields);  /*获取表单的数据*/
        //
        //console.log(files);  /*图片上传成功返回的信息*/
        var title=fields.title[0];
        var price=fields.price[0];
        var fee=fields.fee[0];
        var description=fields.description[0];
        var pic=files.pic[0].path;
        //console.log(pic);

        DB.insert('product',{
            title:title,
            price:price,
            fee,
            description,
            pic

        },function(err,data){
            if(!err){
                res.redirect('/product'); /*上传成功跳转到首页*/
            }

        })
    });
})
```

## 注意

注意图片上传后的路径查找，在上面的图片上传中 `var pic=files.pic[0].path` 获取的图片路径比如 `upload/Aj8HIYV_EqbkNdCNwkYJiL33.jpg` 当我们要显示图片时通过该路径寻找不到图片，原因是获取的图片路径并非真的路径，`upload/Aj8HIYV_EqbkNdCNwkYJiL33.jpg` 是字符串。如果我们设置了静态资源 `app.use(express.static('public'))` 会发现找不到，我们再加上 `app.use(express.static('upload'))` 发现也还是找不到，因为它会在 `upload` 下查找 `upload/Aj8HIYV_EqbkNdCNwkYJiL33.jpg`，所以肯定找不到。

解决方法

```js
// upload/Aj8HIYV_EqbkNdCNwkYJiL33.jpg
app.use('/upload',express.static('upload')) // 配置虚拟路径

// 或者在保存图片路径时，截取字符串为 Aj8HIYV_EqbkNdCNwkYJiL33.jpg
app.use(express.static('upload'))
```

---

通过**自增长**的 `_id` 获取数据

通过数据库存储的 `_id` 拿数据

```js
// db.js
var ObjectID = require('mongodb').ObjectID
exports.ObjectID = ObjectID
```

在查询数据的时候

```js
DB.find('product',{"_id":new DB.ObjectID(id)},function(err,data){})
```

---

修改 form 表单数据(不包括图片时)，也就是不修改图片的时候，插件也会自动创建一个空的文件上传，导致真实的图片数据被覆盖

```js
app.post('/doProductEdit',function(req,res){
    var form = new multiparty.Form();

    form.uploadDir='upload'  // 上传图片保存的地址

    form.parse(req, function(err, fields, files) {

        //获取提交的数据以及图片上传成功返回的图片信息

        //console.log(fields);
        console.log(files);

        var _id=fields._id[0];   /*修改的条件*/
        var title=fields.title[0];
        var price=fields.price[0];
        var fee=fields.fee[0];
        var description=fields.description[0];


        var originalFilename=files.pic[0].originalFilename;
        var pic=files.pic[0].path;

        if(originalFilename){  /*修改了图片*/
            var editData={
                    title,
                    price,
                    fee,
                    description,
                    pic
            };
        }else{ /*没有修改图片，数据库图片地址不动*/
            var editData={
                title,
                price,
                fee,
                description
            };
            //删除生成的临时文件
            fs.unlink(pic);
        }

        DB.update('product',{"_id":new DB.ObjectID(_id)},editData,function(err,data){
                if(!err){
                    res.redirect('/product');
                }
        })
    });
})
```

注意其中修改的 id 是通过设置隐藏的表单携带 id 信息 `<input type="hidden" name="_id" value="<%=list._id%>"/>`