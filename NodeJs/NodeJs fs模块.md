# NodeJs fs模块

```js
var fs=require('fs');
```

## 模块方法

**`1. fs.stat`**

检测是文件还是目录

```js
fs.stat('html',function(err,stats){
 if(err){
   console.log(err);
   return false;
 }
 console.log('文件：'+stats.isFile());
 console.log('目录：'+stats.isDirectory());
})


fs.stat('index.txt',function(err,stats){
if(err){
 console.log(err);
 return false;
}
console.log('文件：'+stats.isFile());
console.log('目录：'+stats.isDirectory());
})
```

**`2. fs.mkdir`**

创建目录

```js
//接收参数：
//path     将创建的目录路径
//mode     目录权限（读写权限），默认0777
//callback 回调，传递异常参数err

fs.mkdir('css',function(err){
 if(err){
  console.log(err);
  return false;
 }
console.log('创建目录成功');
})
```

**`3. fs.writeFile`**

创建写入文件

```js
//filename   (String)   文件名称
//data   (String | Buffer)  将要写入的内容，可以是字符串或 buffer数据。
//options  (Object)   option数组对象，包含：
 //· encoding   (string)  可选值，默认 ‘utf8′，当data使buffer时，该值应该为 ignored。
 //· mode  (Number)  文件读写权限，默认值 438
 //· flag  (String)  默认值 ‘w'
//callback {Function}  回调，传递一个异常参数err。

fs.writeFile('t.txt','你好nodejs 覆盖','utf8',function(err){
  if(err){
   console.log(err);
   return false;
  }
 console.log('写入成功');
})
```

**`4. fs.appendFile`**

追加文件

```js
fs.appendFile('t1.txt','这是写入的内容',function(err){
  if(err){
   console.log(err);
   return false;
  }
 console.log('写入成功');
})

fs.appendFile('t1.txt','这是写入的内容111\n',function(err){
if(err){
 console.log(err);
 return false;
}
console.log('写入成功');
})
```

**`5. fs.readFile`**

读取文件

```js
fs.readFile('t1.txt',function(err,data){
  if(err){
   console.log(err);
   return false;
  }
  //console.log(data);
  console.log(data.toString());
})
```

**`6. fs.readdir`**

读取目录，把目录下面的文件和文件夹都获取到。

```js
fs.readdir('html',function(err,data){
   if(err){
    console.log(err);
    return false;
  }
  console.log(data);
})
```

**`7. fs.rename`**

重命名（改名、剪切文件）

```js
fs.rename('html/index.html','html/news.html',function(err){
   if(err){
    console.log(err);
    return false;
  }
  console.log('修改名字成功');
})

fs.rename('html/css/basic.css','html/style.css',function(err){
    if(err){
     console.log(err);
     return false;
   }
   console.log('剪切成功');
})
```

**`8. fs.rmdir`**

删除目录

```js
fs.rmdir('t',function(err){
     if(err){
      console.log(err);
      return false;
    }
   console.log('删除目录成功');
})

// ENOENT: no such file or directory, rmdir      rmdir 这个方法只能删除目录
fs.rmdir('index.txt',function(err){
     if(err){
      console.log(err);
      return false;
    }
   console.log('删除目录成功');
})
```

**`9. fs.unlink`**

删除文件

```js
fs.unlink('index.txt',function(err){
    if(err){
        console.log(err);
        return false;
     }
    console.log('删除文件成功');
})
```

**`10. fs.createReadStream`**

从文件流中读取数据

```js
const fs = require('fs')

var fileReadStream = fs.createReadStream('data.json')

let count=0; var str='';

fileReadStream.on('data', (chunk) => {
console.log(`${ ++count } 接收到:${chunk.length}`);
str +=chunk })

fileReadStream.on('end', () => { console.log('--- 结束 ---'); console .log(coun t );
console .log(str ); })

fileReadStream.on('error', (error) => { console .log(error)
})
```

**`11. fs.createWriteStream`**

写入文件

```js
var fs = require("fs");

var data = '我是从数据库获取的数据，我要保存起来';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt'); // 使用 utf8 编码写入数据

writerStream .write(data ,'UTF8' ); // 标记文件末尾

writerStream .end();

// 处理流事件 --> finish 事件
writerStream.on('finish', function() { /*finish - 所有数据已被写入到底层系统时触发。*/ console .log("写入完 成。" );
});

writerStream.on('error', function(err){
console.log(err.stack); });
console .log("程序执 行完毕" );
```
**`12. 管道流`**

管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中。

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/NodeJs/images/WX20190313-204321@2x.png)

如上图所示，我们把文件比作装水的桶，而水就是文件的内容，我们用一根管子(pipe)连接两个桶使得水从一个桶流入另一个桶，这样就慢慢的实现了大文件的复制过程。

```js
var fs = require("fs");
// 创建一个可读流
var readerStream = fs.createReadStream('input.txt'); // 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中 
readerStream .pipe(writerStream );
console .log("程 序执行完毕" );
```

## 示例

```js
var fs = require('fs')

// 判断服务器上面有没有 upload 目录，没有创建这个目录（图片上传）

fs.stat('upload', function(err, stats) {
    if(err) { // 没有这个目录
        fs.mkdir('upload', function(err) {
            if(err) {
                console.log(err)
                return false
            }
            console.log('upload 目录创建成功!')
        })
    } else {
        if(stats.isDirectory) {
            console.log('upload 目录存在！')
        }
    }
})

var filesArr = []
// 找出目录下面的所有的目录，然后打印出来
fs.readdir('./', function(err, files) {
    if(err) {
        console.log(err)
    } else {
        // console.log(files)
        // 循环判断是目录还是文件 ---异步，错误写法！！！
        // for(var i = 0; i < files.length; i++) {
        //     fs.stat(files[i], function(err, stats) {
                    // console.log(files[i])  ---undefine
        //     })
        // }

        /*
        匿名自执行函数
        即定义和调用合为一体。我们创建了一个匿名函数，并立即执行它，由于外部无法引用它内部的变量，
        因此在执行完后很快就会被释放。关键是这种机制不会污染全局对象！
        形式：
        (function () {code} ()) ---推荐使用这个
        (function () {code}) ()
        */
        (function getFile(i) {
            // 循环结束
            if(i == files.length) {
                console.log(filesArr)
                return false
            }
            fs.stat('./'+files[i], function(err, stats) {
                if(stats.isDirectory()) {
                    filesArr.push(files[i])
                }
                // 递归调用
                getFile(i+1)
            })
        })(0)
    }
})
```