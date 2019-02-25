# js 中遍历对象与数组

## 遍历对象方法

**1.`for...in`**

```js
var obj = { 'name': "yayaya", 'age': '12', 'sex': 'female' };
Object.prototype.pro1 = function() {};//在原型链上添加属性
Object.defineProperty(obj, 'country', {
  Enumerable: true //可枚举
});
Object.defineProperty(obj, 'nation', {
  Enumerable: false //不可枚举
})
obj.contry = 'china';
for (var index in obj) {
  console.log('key=', index, 'value=', obj[index])
}
```

输出结果

```
key= name value= yayaya
key= age value= 12
key= sex value= female
key= contry value= china
key= pro1 value=function() { … }
```

遍历输出对象自身的属性以及原型链上可枚举的属性，原型链上的属性最后输出。**先遍历的是自身的可枚举属性，后遍历原型链上的。**

**2.`Object.keys()`**

```js
var obj = { 'name': "yayaya", 'age': '12', 'sex': 'female' };
Object.prototype.pro1 = function() {}
Object.defineProperty(obj, 'country', {
  Enumerable: true,
  value: 'ccc'
});
Object.defineProperty(obj, 'nation', {
  Enumerable: false //不可枚举
})
obj.contry = 'china';
Object.keys(obj).forEach(function(index) {
  console.log(index, obj[index])
});
```

输出结果

```
name yayaya
age 12
sex female
contry china
```

遍历对象返回的是一个包含对象自身可枚举属性的数组。

**3.`Object.getOwnPropertyNames()`**

```js
var obj = { 'name': "yayaya", 'age': '12', 'sex': 'female' };
Object.prototype.pro1 = function() {}
Object.defineProperty(obj, 'country', {
  Enumerable: true,
  value: 'ccc'
});
Object.defineProperty(obj, 'nation', {
  Enumerable: false //不可枚举
})
obj.contry = 'china';
Object.getOwnPropertyNames(obj).forEach(function(index) {
  console.log(index, obj[index])
});
```
输出结果

```
name yayaya
age 12
sex female
country ccc
nation undefined
contry china
```

输出对象自身的可枚举和不可枚举属性的数组，不输出原型链上的属性。

**4.`Reflect.ownKeys()`**

```js
var obj = { 'name': "yayaya", 'age': '12', 'sex': 'female' };
Object.prototype.pro1 = function() {}
Object.defineProperty(obj, 'country', {
  Enumerable: true,
  value: 'ccc'
});
Object.defineProperty(obj, 'nation', {
  Enumerable: false //不可枚举
})
obj.contry = 'china';
Reflect.ownKeys(obj).forEach(function(index) {
  console.log(index, obj[index])
});
```
输出结果

```
name yayaya
age 12
sex female
country ccc
nation undefined
contry china
```

返回对象自身的所有属性，不管是否可枚举，不管属性名是 Symbol 或字符串


## 遍历数组方法

**1.`forEach`**

```js
var arr = ['a', 'b', 'c', 'd'];
arr.forEach(function(value, index) {
  console.log('value=', value, 'index=', index);
})
```
输出结果

```
value= a index= 0
value= b index= 1
value= c index= 2
value= d index= 3
```

**2.`map`**

```js
var arr = ['a', 'b', 'c', 'd'];
arr.map(function(item, index, array) {
  console.log(item, index);
})
```
输出结果

```
a 0
b 1
c 2
d 3
```

**3.`for`**

```js
var arr = ['a', 'b', 'c', 'd'];
for (var i = 0; i < arr.length; i++) {
  console.log(i, arr[i])
}
```
输出结果

```
0 a
1 b
2 c
3 d
```

**4.`for...in`**

```js
var arr = ['a', 'b', 'c', 'd'];
for (var i in arr) {
  console.log('index:', i, 'value:', arr[i])
}
```
输出结果

```
0 a
1 b
2 c
3 d
```

**5.`for...of`**

```js
var arr = ['a', 'b', 'c', 'd'];
for (var value of arr) {
  console.log('value', value)
}
```
输出结果

```
value a
value b
value c
value d
```

http://www.php.cn/js-tutorial-408347.html