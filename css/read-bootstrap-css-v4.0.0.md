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

