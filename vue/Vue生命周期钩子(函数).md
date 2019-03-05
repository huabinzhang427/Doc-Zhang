# Vue生命周期钩子(函数)

## 概念

生命周期函数：**组件挂载、更新以及销毁的时候触发的一系列的方法**。

它是`每个 Vue 实例`在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、`将实例挂载到 DOM` 并在数据变化时更新 DOM 等。

## 生命周期函数

代码演示

创建一个演示的组件

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-152100@2x.png)

将演示组件引用到根组件中

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-152626@2x.png)

在浏览器中演示说明

刷新页面(挂载按钮)

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-153031@2x.png)

点击更新数据按钮

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-152959@2x.png)

点击卸载按钮

![image](https://github.com/huabinzhang427/Doc-Zhang/blob/master/vue/images/WX20190305-152853@2x.png)

## 总结

重点，注意生命周期函数中的几个重要的：

* `mounted()`，请求数据放在这个方法里！！！
* `beforeDestroy(）`，页面销毁的时候要保存一些数据在这个方法里处理

理解，**生命周期函数是对每个组件而言，也就是每个 Vue 实例！**