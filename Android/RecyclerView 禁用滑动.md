需求：试卷功能，每个 Item 一道题，禁止 Item 左右滑动，下方有按钮“上一题”和“下一题”来切换 Item 显示不同的考题。

## 操作

**1次操作**：

```java
recyclerView.setNestedScrollingEnabled(false);
```

问题：无效，RecyclerView 中的 Item 还是可以左右滑动。

**2次操作**：

```java
manager = new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL,  
                false) {  
            @Override  
            public boolean canScrollHorizontally() {  
                return false;  
            }  
        };  
testList.setLayoutManager(manager); 
```

需求完成，RecyclerView 的 Item 滑动禁用成功。

## 分析

操作1中的 `setNestedScrollingEnabled()` 方法是处理 ScrollView 嵌套 RecyclerView 中遇到页面卡顿，内容显示等问题。

```java
recyclerView.setHasFixedSize(true);
recyclerView.setNestedScrollingEnabled(false);
```

`setHasFixedSize(true)` 方法使得 RecyclerView 能够固定自身 size 不受 adapter 变化的影响；

`setNestedScrollingeEnabled(false)` 方法则是进一步调用了 RecyclerView 内部 `NestedScrollingChildHelper` 对象的 `setNestedScrollingeEnabled(false)` 方法：

```java
public void setNestedScrollingEnabled(boolean enabled) {
    getScrollingChildHelper().setNestedScrollingEnabled(enabled);
}
```

进而，`NestedScrollingChildHelper` 对象通过该方法关闭 RecyclerView 的嵌套滑动特性：

```java
public void setNestedScrollingEnabled(boolean enabled) {
    if (mIsNestedScrollingEnabled) {
        ViewCompat.stopNestedScroll(mView);
    }
    mIsNestedScrollingEnabled = enabled;
}
```

如此一来，限制了 RecyclerView 自身的滑动，整个页面滑动仅依靠 ScrollView 实现，即可解决滑动卡顿的问题。

对于不存在嵌套问题的，使用 `setNestedScrollingEnabled()` 无效。

通过重写 `LayoutManager`：

```java
LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this) {
    @Override
    public boolean canScrollHorizontally() {
        return false;
    }
};
```

该方式使得 RecyclerView 的垂直滑动始终返回 false，其目的同样是为了限制自身的滑动。


参考文章

[https://segmentfault.com/a/1190000011553735](https://segmentfault.com/a/1190000011553735)

