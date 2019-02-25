# Read Bootstrap.css v4.0.0

1、

`:root`选择器，匹配文档的根元素。

举例：设置 HTML 文档的背景色

```css
:root{
    background: #ff0000
}
```
https://developer.mozilla.org/zh-CN/docs/Web/CSS/:root

---

2、

**使用CSS变量**

CSS 变量是由 CSS 作者定义的实体，其中包含要在整个文档中重复使用的特定值。使用自定义属性来设置变量名，并使用特定的  `var()` 来访问。（比如  `color: var(--main-color);`）。

基本用法

```css
/**声明一个局部变量*/
element {
    --main-bg-color: brown;
}
/**使用一个局部变量*/
element {
   background-color: var(--main-bg-color);
}

/**声明一个全局变量*/
:root {
    --global-color: #666;
    --pane-padding: 5px 42px;
}
/**使用一个全局变量*/
.demo {
    color: var(--global-color)
}
```

解决的问题

**维护性**，在构建大型站点时，作者通常会面对可维护性的挑战。在这些网页中， 所使用的 CSS 的数量是非常庞大的，并且在许多场合大量的信息会重复使用。例如，在网页中维护一个配色方案，意味着一些颜色在CSS文件中多次出现，并被重复使用。当你修改配色方案时，不论是调整某个颜色或完全修改整个配色，都会成为一个复杂的问题，不容出错，而单纯查找替换是远远不够的。

**通俗性**，这些变量的第二个优势就是名称本身就包含了语义的信息。CSS 文件变得易读和理解。`main-text-color` 比文档中的 `#00ff00` 更容易理解，特别是同样的颜色出现在不同的文件中的时候。

https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_variables

---
3、

**`::before/::after`**

`::before` 和 `::after` 创建一个**伪元素**，其将成为匹配选中的元素的**第一个子元素**。常通过 `content` 属性来为一个元素添加修饰性的内容。此元素**默认为行内元素**。

举例1：使用 `::before` 和 `::after` 来插入引用性文本。

```html
<q>一些引用</q>, 他说, <q>比没有好。</q>.
```

```css
q::before { 
  content: "«";
  color: blue;
}
q::after { 
  content: "»";
  color: red;
}

/**  «一些引用», 他说, «比没有好。». **/
```

举例2：使用伪元素来创建一个简单的待办列表。

```html
<ul>
  <li>Buy milk</li>
  <li>Take the dog for a walk</li>
  <li>Exercise</li>
  <li>Write code</li>
  <li>Play music</li>
  <li>Relax</li>
</ul>
```

```css
li {
  list-style-type: none;
  position: relative;
  margin: 2px;
  padding: 0.5em 0.5em 0.5em 2em;
  background: lightgrey;
  font-family: sans-serif;
}

li.done {
  background: #CCFF99;
}

li.done::before {
  content: '';
  position: absolute;
  border-color: #009933;
  border-style: solid;
  /*   边框宽度 0 没有，形成对勾状 */
  border-width: 0 0.3em 0.25em 0;
  height: 1em;
  top: 1.3em;
  left: 0.6em;
  margin-top: -1em;
  /*  倾斜角度  */
  transform: rotate(45deg);
  width: 0.5em;
}
```

```js
var list = document.querySelector('ul');
list.addEventListener('click', function(ev) {
  if( ev.target.tagName === 'LI') {
     ev.target.classList.toggle('done'); 
  }
}, false);
```
注意这里我们没有使用任何图标，对勾标识实际上是使用 CSS 定义了样式的 `::before` 伪元素。

---

4、

**positon 绝对位置（absolute、relative）**

**设置对象的定位方式**，可以让布局层容易位置绝对定位，控制盒子对象更加准确。

绝对定位使用条件

`position:absolute；`、`position:relative` 绝对定位使用通常是父级定义 `position:relative` 定位，子级定义 `position:absolute` 绝对定位属性，并且子级使用 `left` 或 `right` 和 `top` 或 `bottom` 进行绝对定位。

需要注意的是，`left`（左）和 `right`（右）在一个对象只能选一种定义，`bottom`（下）和 `top`（上）也是在一个对象只能选一种定义。

举例：设置一个最外层盒子 css 边框为红色，css width 为 400px, css height 为 200px,内部包含了2个盒子，为就用绝对定位这2个盒子，第一个盒子 CSS 命名为 “divcss5-a”,其宽度为 100px,背景颜色为黑色，高度为 100px,定位距离父级上为 10px，距离左为 10px;第二个盒子 CSS 类命名为 “divcss5-b”，其宽度和高度分别为 50px,css 背景颜色为蓝色，距离父级下距离为 13px,距离父级右边为 15px。

```css
.divcss5{ position:relative;width:400px;height:200px; 
border:1px solid #000} 
/* 定义父级position:relative 为就认为是绝对定位声明吧 */ 
.divcss5-a{ position:absolute;width:100px;height:100px; 
left:10px;top:10px;background:#000} 
/* 使用绝对定位position:absolute样式 并且使用left top进行定位位置 */ 
.divcss5-b{ position:absolute;width:50px;height:50px; 
right:15px;bottom:13px;background:#00F} 
/* 使用绝对定位position:absolute样式 并且使用right bottom进行定位位置 */ 
```

```html
<div class="divcss5"> 
    <div class="divcss5-a"></div> 
    <div class="divcss5-b"></div> 
</div>
```

http://www.divcss5.com/rumen/r403.shtml

---

5、

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```
`*` 是所有元素。上述代码段表示**所有元素**以及**所有元素内容之前**以及**内容之后**都继承父元素的 `box-sizing` 属性。

---

6、

**盒模型box-sizing**

规定边框、内边距与元素的实际大小（包含/不包含）。

* `content-box`（默认）：宽度和高度分别应用到元素的内容框。**在宽度和高度之外绘制元素的内边距和边框**。
* `border-box`：为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。

```css
#element {
    width: 208px;
    padding: 32px;
    border: 16px;
}
```

`图片`

https://www.cnblogs.com/ooooevan/p/5470982.html

---

7、

`text-size-adjust: 100%;`

当将字体设置为小于12px的时候，chrome浏览器往往不会显示比 **12px** 还要小的字，因为该浏览器中默认的字体最小是 12px，所以当你想设置为 10px 的时候，该浏览器页面中还是会显示的是 12px 的大小的。但是如果我们想要把它设置为 10px 字体的话，我们就可以利用这个属性了。

但是用这个属性，并且是放在 html 中或者是 body 中的，会导致页面的缩放效果失效，而且当我们想要放大整个页面的时候，一般来说字体也应该是要放大的，但是就因为我们使用了该属性，导致字体是不会随着网页的放大而变大的。

```css
/*  通常来说我们都是想让哪一个浏览器设置这个属性就加上哪一个浏览器内核的前缀： */
html{-webkit-text-size-adjust:none}
/* 建议下面的写法 */
html{-webkit-text-size-adjust:100%;}
```

---

8、

display 属性

该属性规定元素应该生成的框的类型。

```css
p{
  display: inline;
}
```
* `inline`（默认）：此元素会被显示为内联元素，元素前后没有换行符
* `inline-block`（融合行内于块级）：不独占一行的块级元素
* `none`：此元素不会被显示
* `block`：此元素将显示为块级元素，此元素前后会带有换行符

举例：

```html
<html>
<head>
<style type="text/css">
p {display: inline}
div {display: none}
</style>
</head>

<body>
<p>本例中的样式表把段落元素设置为内联元素。</p>

<p>而 div 元素不会显示出来！</p>

<div>div 元素的内容不会显示出来！</div>
</body>
</html>
```

输出结果：

```
本例中的样式表把段落元素设置为内联元素。 而 div 元素不会显示出来！
```

```html
<html>
<head>
<style type="text/css">
span
{
display: block
}
</style>
</head>
<body>

<span>本例中的样式表把 span 元素设置为块级元素。</span>
<span>两个 span 元素之间产生了一个换行行为。</span>

</body>
</html>

```

内联元素就是元素间不换行，块级元素就是每个元素在各自的行内！


http://www.w3school.com.cn/cssref/pr_class_display.asp

---

9、

outline 属性

该属性是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

```css
p {
   outline:#00FF00 dotted thick;
}
```

* outline-color：边框的颜色
* outline-style：边框的样式(none-无轮廓、dotted-点状、dashed-虚线、solid-实线等)
* outline-width：边框的宽度

http://www.w3school.com.cn/cssref/pr_outline.asp

outline-offset 属性

该属性对轮廓进行偏移（轮廓与边框边缘的距离），并在边框边缘进行绘制。

举例：规定边框边缘之外 15 像素处的轮廓

```css
div{
  border:2px solid black;
  outline:2px solid red;
  outline-offset:15px;
}
```

---

9、

font-style 属性

该属性设置使用斜体、倾斜或正常字体。

* normal（默认值）：标准字体样式。
* italic：斜体
* oblique：倾斜

---

10、

vertical-align 属性

该属性设置元素的**垂直对齐方式**。这个属性会设置**单元格框中的单元格内容的对齐方式**。

* baseline（默认）：元素放置在父元素的基线上
* sub：垂直对齐文本的下标
* super：垂直对齐文本的上标
* top：把元素的顶端与行中最高元素的顶端对齐
* text-top：把元素的顶端与父元素字体的顶端对齐
* middle：把此元素放置在父元素的中部
* bottom：把元素的顶端与行中最低的元素的顶端对齐
* text-bottom：把元素的底端与父元素字体的底端对齐

http://www.w3school.com.cn/css/pr_pos_vertical-align.asp

---

11、

text-decoration 属性

这个属性允许对文本设置某种效果，如**加下划线**。如果后代元素没有自己的装饰，祖先元素上设置的装饰会“延伸”到后代元素中。

* none（默认）：标准的文本
* underline：文本下的一条线
* overline：文本上的一条线
* line-through：穿过文本的一条线
* blink：闪烁的文本

http://www.w3school.com.cn/cssref/pr_text_text-decoration.asp

---

12、

overflow 属性

该属性规定当内容溢出元素框时发生的事情。

* visible（默认）：内容不会被修剪，会呈现在元素框之外
* hidden：内容会被修剪，并且其余内容是不可见的
* scroll：内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容
* auto：如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容


http://www.w3school.com.cn/cssref/pr_pos_overflow.asp

overflow-style 属性

该属性规定溢出元素的首选滚动方法。

* auto
* scrollbar：为溢出元素添加滚动条
* panner
* move：用户能够直接移动元素的内容。通常，用户能够用鼠标拖动内容
* marquee：内容自主移动，不需任何用户代理对其控制

http://www.w3school.com.cn/cssref/pr_overflow-style.asp

---

13、

caption-side 属性

该属性设置表格标题的位置。指定了表标题相对于表框的放置位置。表标题显示为好像它是表之前（或之后）的一个块级元素。

* top（默认）：表格标题定位在表格之上
* bottom：表格标题定位在表格之下
* inherit：规定应该从父元素继承 caption-side 属性的值

http://www.w3school.com.cn/cssref/pr_tab_caption-side.asp

---

14、

text-transform 属性

该属性控制文本的大小写。

* none（默认）：标准文本
* capitalize：文本中的每个单词以大写字母开头
* uppercase：定义仅有大写字母
* lowercase：定义无大写字母，仅有小写字母

http://www.w3school.com.cn/cssref/pr_text_text-transform.asp

---

15、

appearance 属性

该属性允许您使元素看上去像标准的用户界面元素。

举例：使 div 元素看上去像一个按钮：

```css
div
{
appearance:button;
-moz-appearance:button; /* Firefox */
-webkit-appearance:button; /* Safari 和 Chrome */
}
```

所有主流浏览器都不支持 appearance 属性。Firefox 支持替代的 -moz-appearance 属性。Safari 和 Chrome 支持替代的 -webkit-appearance 属性。

* normal：将元素呈现为常规元素
* icon：将元素呈现为图标（小图片）
* window： 将元素呈现为视口
* button：将元素呈现为按钮
* menu： 将元素呈现为一套供用户选择的选项
* field：将元素呈现为输入字段

http://www.w3school.com.cn/cssref/pr_appearance.asp

---

16、

white-space 属性

该属性设置如何处理元素内的空白。

举例：规定段落中的文本不进行换行

```css
p{
   white-space: nowrap;
}
```

* normal（默认）:空白会被浏览器忽略
* pre：空白会被浏览器保留。其行为方式类似 HTML 中的 `<pre>` 标签
* nowrap：文本不会换行，文本会在在同一行上继续，直到遇到 `<br>` 标签为止
* pre-wrap：保留空白符序列，但是正常地进行换行
* pre-line：合并空白符序列，但是保留换行符

http://www.w3school.com.cn/cssref/pr_text_white-space.asp

---

17、

cursor 属性

该属性规定要显示的光标的类型（形状）。定义了鼠标指针放在一个元素边界范围内时所用的光标形状。

http://www.w3school.com.cn/cssref/pr_class_cursor.asp


---

18、

单位 px、em 和 rem

* px；绝对单位
* em：相对单位，**基准点是父节点的大小**，如果自身定义了 `font-size` 按自身来计算（浏览器默认字体为 16px）
* rem：相对单位，可理解为（`root em`），相对根节点 html 字体大小来计算，css3 新加属性

---

19、

opacity 属性

该属性设置元素的**不透明级别**。

* value：规定不透明度。从 0.0 （完全透明）到 1.0（完全不透明）
* inherit：从父元素继承 opacity 属性的值

---

20、

list-style 属性

该属性设置所有的列表样式。

* list-style-type：none、disc（默认）-实心圆、circle-空心圆、decimal-数字等
* list-style-position：inside、outside（默认）
* list-style-image：none、url('图像路径')


http://www.w3school.com.cn/cssref/pr_list-style.asp

---