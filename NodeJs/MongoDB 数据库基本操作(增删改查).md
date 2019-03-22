# MongoDB 数据库基本操作(增删改查)

开启 mongodb 服务

```
mongod --dbpath c:\mongodb
```

管理 mongodb 数据库，建立连接

```
mongo
```

* `cls`-清屏
* `show dbs`-查看所有数据库列表

## 创建/使用数据库

```
use student
```
`student`-为举例数据库名称，如果真的想把这个数据库创建成功，那么必须插入一个数据。

数据库中不能直接插入数据，只能往集合(collections)中插入数据。不需要专门创建集合，只需要写点语法插入数据就会创建集合:

```
db.student.insert({“name”:”x iaom ing”});
```
db.student 系统发现 student 是一个陌生的集合名字，所以就自动创建了集合。

显示当前的数据集合(mysql 中叫表)

```
show collections
```

删除数据库，删除当前所在的数据库

```
db.dropDatabase();
```

删除集合，删除指定的集合、删除表

```
db.student.drop()
```

## 插入(增加)数据

插入数据，随着数据的插入，数据库创建成功了，集合也创建成功了。

```
db.student.insert({"name":"zhangsan"})
```

## 查找数据

* 查询所有记录

```
db.student.find()

查询第一条数据
db.student.findOne( );
```
相当于: `select* from userInfo`

* 查询当前集合中的某列的重复数据去掉后的

```
db.student.distinct("name");
```
会过滤掉 name 中的相同数据，相当于:`select distict name from userInfo`

* 条件查询

```
查询 age = 22 的记录
db.student.find({"age": 22})

查询 age > 22 的记录
db.student.find({"age": {$gt: 22}})

查询 age < 22 的记录
db.student.find({"age": {$lt: 22}})

查询 age >= 25 的记录
db.student.find({"age": {$gte: 25}})

查询 age <= 25 的记录
db.student.find({"age": {$lte: 25}})

查询 age >= 23 并且 age <= 26  注意书写格式
db.student.find({"age": {$gte: 23, $lte: 26}})

查询 name = zhangsan, age = 22 的数据
db.student.find({"name": 'zhangsan', "age": 22})
```

```
查询name中包含 mongo的数据  模糊查询用于搜索
db.student.find({"name": /mongo/})

查询 name 中以 mongo 开头的
db.student.find({"name": /^mongo/})
```

```
查询指定列 name、age 数据
db.student.find({}, {name: 1, age: 1})
当然 name 也可以用 true 或 false,当用 ture 的情况下河 name:1 效果一样，如果用 false 就 是排除 name，显示 name 以外的列信息。

查询指定列 name、age 数据, age > 25
db.student.find({age: {$gt: 25}}, {name: 1, age: 1});

按照年龄排序 1 升序 -1 降序
db.student.find().sort({age: 1})
db.student.find().sort({age: -1})
```

```
分页查询应用(limit 是 pageSize，skip 是第几页*pageSize)
查询前 5 条数据
db.student.find().limit(5)

查询 10 条以后的数据
db.student.find().skip(10)

查询在 5-10 之间的数据
db.student.find().limit(10).skip(5)
```

```
or-与 查询
db.student.find({$or: [{age: 22}, {age: 25}]})
```

* 查询某个结果集的记录条数---统计数量

```
db.student.find({age: {$gte: 25}}).count()

如果要返回限制之后的记录数量，要使用 count(true)或者 count(非 0)
db.student.find().skip(10).limit(5).count(true)
```

## 修改数据

修改里面还有查询条件。

```
查找名字叫做小明的，把年龄更改为 16 岁
db.student.update({"name":"小明"},{$set:{"age":16}})

查找数学成绩是 70，把年龄更改为 33 岁
db.student.update({"score.shuxue":70},{$set:{"age":33}})

更改所有匹配项目
db.student.update({"sex":"男"},{$set:{"age":33}},{multi: true})

完整替换
db.student.update({"name":"小明"},{"name":"大明","age":16})
```

## 删除数据

```
默认该方法会移除所有的匹配条件的记录
db.student.remove({"age": 132});

移除所有的匹配条件的一条记录
db.student.remove({"name": "小米"}, {justOne: true})
```

