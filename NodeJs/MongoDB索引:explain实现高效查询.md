# MongoDB索引、explain实现高效查询

## 索引

### 索引基础

索引是对数据库表中一列或多列的值进行排序的一种结构，可以让我们查询数据库变得更快。MongoDB 的索引几乎与传统的关系型数据库一模一样，这其中也包括一些基本的查询优化技巧。

* 创建索引

```
db.user.ensureIndex({"username":1})
```

* 获取当前集合的索引

```
db.user.getIndexes()
```

* 删除索引

```
db.user.dropIndex({"username":1})
```

在 MongoDB 中，我们同样可以创建**复合索引**

数字 1 表示 username 键的索引按**升序**存储，-1 表示 age 键的索引按照**降序**方式存储。

```
db.user.ensureIndex({"username":1, "age":-1})
```

**随着集合的增长，需要针对查询中大量的排序做索引。如果没有对索引的键调用 sort， MongoDB 需要将所有数据提取到内存并排序。因此在做无索引排序时，如果数据量过大以致无法在内存中进行排序，此时 MongoDB 将会报错。**

### 唯一索引

在缺省情况下创建的索引均不是唯一索引。

```
db.user.ensureIndex( {"userid":1},{"unique":true})
```

在缺省情况下创建的索引均不是唯一索引。

```
db.user.insert({"userid":5}) 
db.user.insert({"userid":5})

E11000 duplicate key error index: user.user.$userid_1 dupkey: {:5.0 }
```

如果插入的文档中不包含 userid 键，那么该文档中该键的值为 null，如果多次插入类似 的文档，MongoDB 将会报出同样的错误，如:

```
db.user.insert({"userid1":5}) 
db.user.insert({"userid1":5})

E11000 duplicate key error index: user.user.$userid_1 dupkey: {:null }
```

如果在创建唯一索引时已经存在了重复项，我们可以通过下面的命令帮助我们在创建唯 一索引时消除重复文档，仅保留发现的第一个文档，如:

```
先删除刚刚创建的唯一索引
db.user.dropIndex({"userid":1})

插入测试数据，以保证集合中有重复键存在
db.user.remove() db.user.insert({"userid":5})
db.user.insert({"userid":5})

重新创建唯一索引
db.user.ensureIndex({"userid":1},{"unique":true })
```

我们同样可以创建复合唯一索引，即保证复合键值唯一即可。

```
db.user.ensureIndex({"useri d":1,"age":1},{"unique":true})
```

### 索引的一些参数

* `background`，Boolean，建索引过程会阻塞其它数据库操作，background 可指定以后台方式创建索引，即增加 background 可选参数，默认为 false。
* `unique`，Boolean，建立的索引是否唯一。指定为 true 创建唯一索引，默认为 false。
* `name`，String，索引的名称，如果未指定，MongoDB 通过连接索引的字段名和排序顺序生成一个索引名称。
* `dropDups`，Boolean，在建立唯一索引时是否删除重复记录，指定 true 创建唯一索引，默认为 false。

```
db.user.ensureIndex( {"username":1},{"background":true})
```

如果在为已有数据的文档创建索引时，可以执行下面的命令，以使 MongoDB 在后台创建索引，这样的创建时就不会阻塞其他操作。但是相比而言，以阻塞方式创建索引，会使整个创建过程效率更高，但是在创建时 MongoDB 将无法接收其他的操作。

## 使用 explain

explain 是非常有用的工具，会帮助你获得查询方面诸多有用的信息。只要对游标调用该方法，就可以得到查询细节。explain 会返回一个文档，而不是游标本身。

explain 会返回查询使用的索引情况，耗时和扫描文档数的统计信息。

explain executionStats 查询具体的执行时间

```
db.tablename.find().explain( "executionStats" )
```