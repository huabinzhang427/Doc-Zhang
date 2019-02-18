# JS数据类型转换

`js` 只一中弱类型的脚本语言，语法相对与 `java` 等高级编程语言来说不够严格，所以对于它的数据类型和之间的转换很容易混淆。

## JS中的数据类型

js中的数据类型可以分为五种：number 、string 、boolean、 underfine 、null。

* number:数字类型 ，整型浮点型都包括
* string：字符串类型
* boolean：布尔类型
* underfine:未定义，一般指的是已经声明，但是没有赋值的变量
* null:空对象类型，var a = null，和var a=""有区别

另外，object：对象，对象就是把一些彼此相关的属性和方法集合在一起构成的一个数据实体，在js常见的有window,document,array等。

NaN:他是表示不是数字值的特殊值，可以理解为 Number 的一种特殊类型，只有当在 js 运算中发生数据类型转换时提示，`isNaN()` 方法是特有的对数据进行判断的 ，**如果是数字返回false，不是数字返回true**。


## JS数据类型转换

 在 js 中数据类型转换一般分为两种，即**强制类型转换**和**隐式类型转换**(利用js弱变量类型转换)。

### 强制类型转换

利用 js 提供的函数 `parseInt()` 、`parseFloat()` 、`Number()`、`Boolean()` 进行数据转换，顾名思义，前两个分别是对数据进行解析转换，前者是整数，后者是浮点数。他们解析的原则是从前往后进行解析，尽其所能。若存在有能识别的数字就解析，如果第一位就不是数字则返回 NaN。Number则是对整体进行判断，是数字返回数字，否则 NaN。

**`ParseInt()`**

```js
// ParseInt()：
parseInt("123");//123

parseInt("+123");//123

parseInt("-123");//123

parseInt("12.3元")//12

parseInt("abc");//NaN

parseInt([1,2])//1

parseInt([1,2,4])//1

parseInt("  ");//NaN
```
该方法还有**基模式**，可以把二进制、八进制、十六进制或其他任何进制的字符串转换成整数。基是由 `parseInt()` 方法的**第二个参数指定**的。

```js
parseInt("AA",16);//170

parseInt("10",2);//2

parseInt("10",8);//8

parseInt("10",10);//10

//含有前导
parseInt("0xAA")//170        

//如果二进制数包含前导0，那么最好采用基数2，不然默认以十进制解析
parseInt("010");   //returns   10 

parseInt("010",2);//2         

parseInt("010",   8);   //returns   8     

parseInt("010",   10);   //returns   10 

parseInt(null)//NaN
```

**`ParseFloat()`**

```js
parseFloat("123");//123

parseFloat("-123");//123

parseFloat("+123");//123

parseFloat("12.34");//12.34

parseFloat("12.35元")//12.35

parseFloat("12.23.122");//12.23

parseFloat("av");//NaN

parseFloat("0xAA");//0

parseFloat("0110");//110

parseFloat([1]);//1

parseFloat([2,3]);//2

parseFloat([]);//NaN

parseFloat(null)//NaN
```

**`Number()`**

```js
Number("123");//123

Number("+123");//123

Number("12.3");//12.3

Number(true);//1

Number("12.3.4");//NaN

Number(" ");//0

Number("abc");//NaN

Number([]);//0

Number([1]);//1

Number([1,2]);//NaN

Number(new Object());//NaN

Number(null);//0
```

**`Boolean()`**

它不会对引号里面的数字进行自动进行转换。

```js
Boolean(1) ;//true

Boolean(0);//false

Boolean("1");//true

Boolean("0");//true

Boolean("abc");//true

Boolean('');//false

Boolean('   ');//true

Boolean([])//true

Boolean([1])//true

Boolean(null)//false
```


### 隐式类型转换

隐式类型转换和 java 中大不相同，在 js 中数据类型不严格，没有浮点和整型，这里的隐式类型转换指的是 _字符串和数值类型之间的转换_，在进行字符串和数字之间进行 _减乘除取模运算或者进行比较运算时，他会自动把字符串转换为数字_。转换数字的默认方法是调用 Number()，进行 _加法运算则是将数字看成字符串进行拼接_。

```js
var x = "123";

var y = 121;

(x+y);//"123121";

(x-y);//2

(x*y);//14883

(x/y);//1.016528256198346

(x%y);//2

(x>y);//true

(x==y);//false

("123a">y);//false诡异
```

