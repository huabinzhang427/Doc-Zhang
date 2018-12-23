需求：操作A(Activity)->B->C->A，B 和 C 直接干掉！

思考：从 C 返回到 A，会想到先finish C，再进入 B finish B，之后回到 A。但是这样会使得页面体验不好，而且这么写很 low。这就想到了运用 Activity 的启动模式来满足我们的需求。

# 概述

默认情况下，当我们多次启动同一个 Activity 时，系统会创建多个实例，并把它们按照先进后出的原则一一放入任务栈中，当我们按 back 键时，就会有一个 Activity 从任务栈顶移除，重复下去，直到任务栈为空，系统就会回收这个任务栈。

重复创建多个实例（Activity）是不合理的，为了优化这个问题，Android 系统提供了四种启动模式(`standard`、`singleTop`、`singleTask` 和 `singleInstance`)来修改系统这一默认行为。


# standard-默认启动模式

这个模式是默认的启动模式，即标准模式，在不指定启动模式的前提下，系统默认使用该模式启动 Activity，每次启动一个 Activity 都会重写创建一个新的实例，不管这个实例存不存在，这种模式下，`谁启动了该模式的 Activity，该 Activity 就属于启动它的 Activity 的任务栈中`。这个 Activity 它的 `onCreate()`，`onStart()`，`onResume()` 方法都会被调用。 

```java
<activity android:name=".standard.StandardActivity" android:launchMode="standard"/>
```

对于 `standard` 模式，`android:launchMode` 可以不进行声明，因为`默认就是 standard`。

入口 MainActivity 中有一个按钮进入 StandardActivity，StandardActivity 中又有一个按钮启动 StandardActivity。


在 MainActivity 中：

```java
startActivity(new Intent(this, StandardActivity.class));
```

我们进入 StandardActivity 后，再点击按钮进入 StandardActivity，几次操作后，日志如下：


```
12-23 04:04:45.434 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: MainActivity TaskId: 24 hashCode: 1383157648
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:04:50.278 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: StandardActivity TaskId: 24 hashCode: 1383067728
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:04:52.158 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: StandardActivity TaskId: 24 hashCode: 1383467164
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:04:53.682 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: StandardActivity TaskId: 24 hashCode: 1383521488
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:04:54.894 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: StandardActivity TaskId: 24 hashCode: 1383108128
    taskAffinity: com.example.zhanghuabin.activitylunchmode
```

可以看到日志输出了一次 MainActivity 和四次 StandardActivity，从 MainActivity 进入 StandardActivity 一次，后来我们又按了三次按钮，总共四次 StandardActivity 的日志，并且所属的任务栈的 id 都是 24。`谁启动了该模式的 Activity，该 Activity 就属于启动它的 Activity 的任务栈中`。

因为启动 StandardActivity 的是 MainActivity，而 MainActivity 的 taskId 是 24，因此启动的 StandardActivity 也应该属于 id 为 24 的这个 task，后续的 3 个 StandardActivity 是被S tandardActivity 这个对象启动的，因此也应该还是 24，所以 taskId 都是 24。

每一个 Activity 的 hashcode 都是不一样的，说明他们是不同的实例，即“每次启动一个 Activity 都会重写创建一个新的实例”。

standard 启动模式是默认的启动模式，每次启动一个 Activity 都会新建一个实例不管栈中是否已有该 Activity 的实例。


# singleTop-栈顶复用模式

这个模式下，如果新的 activity 已经位于栈顶，那么这个 Activity 不会被重写创建，同时它的 `onNewIntent()` 方法会被调用，通过此方法的参数我们可以去除当前请求的信息。如果栈顶不存在该 Activity 的实例，则情况与 standard 模式相同。需要注意的是复用的 Activity 它的 `onCreate()`，`onStart()` 方法不会被调用，因为它并没有发生改变。 

```java
<activity android:name=".singletop.SingleTopActivity" android:launchMode="singleTop"/>
```

在 MainActivity 中：

```java
startActivity(new Intent(this, SingleTopActivity.class));
```

```
12-23 04:18:47.934 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: MainActivity TaskId: 25 hashCode: 1383418608
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:18:53.034 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: SingleTopActivity TaskId: 25 hashCode: 1383210332
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:18:55.930 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTopActivity TaskId: 25 hashCode: 1383210332
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:19:01.078 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
12-23 04:19:01.082 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: onCreate: StandardActivity TaskId: 25 hashCode: 1383304568
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:19:03.406 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: SingleTopActivity TaskId: 25 hashCode: 1383602132
12-23 04:19:03.410 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:19:05.710 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTopActivity TaskId: 25 hashCode: 1383602132
    taskAffinity: com.example.zhanghuabin.activitylunchmode
```

除了第一次进入 SingleTopActivity 这个 Activity 时，输出的是 `onCreate()` 方法中的日志，后续的都是调用了 `onNewIntent()` 方法，并没有调用 onCreate 方法，并且两个日志的 hashcode 都是一样的，说明`栈中只有一个实例`。

1. 当前栈中已有该 Activity 的实例并且该实例位于栈顶时，不会新建实例，而是复用栈顶的实例，并且会将 Intent 对象传入，回调 `onNewIntent()` 方法
2. 当前栈中已有该 Activity 的实例但是该实例不在栈顶时，其行为和 standard 启动模式一样，依然会创建一个新的实例
3. 当前栈中不存在该 Activity 的实例时，其行为同 standard 启动模式


## taskAffinity属性

* 这个参数标识了一个 Activity 所需任务栈的名字，默认情况下，所有 Activity 所需的任务栈的名字为`应用的包名`
* 我们可以单独指定每一个 Activity 的 taskAffinity 属性覆盖默认值
* 一个任务的 affinity 决定于这个任务的根 activity（root activity）的 taskAffinity
* 在概念上，具有相同的 affinity 的 activity（即设置了相同 taskAffinity 属性的 activity）属于同一个任务（应用）
* 为一个 activity 的 taskAffinity 设置一个空字符串，表明这个 activity 不属于任何 task


很重要的一点 taskAffinity 属性不对 `standard` 和 `singleTop` 模式有任何影响，即时你指定了该属性为其他不同的值，这两种启动模式下不会创建新的task（如果不指定即默认值，即包名）。


# singleTask-栈内复用模式

在这个模式下，如果栈中存在这个 Activity 的实例就会复用这个 Activity，不管它是否位于栈顶，复用时，会将它上面的 Activity 全部出栈，并且会回调该实例的 onNewIntent 方法。其实这个过程还存在一个任务栈的匹配，因为这个模式启动时，会在自己需要的任务栈中寻找实例，这个任务栈就是通过 `taskAffinity` 属性指定。如果这个任务栈不存在，则会创建这个任务栈。 

```java
<activity android:name=".singletask.SingleTaskActivity" android:launchMode="singleTask"/>
```

在 MainActivity 中：

```java
startActivity(new Intent(this, SingleTaskActivity.class));
```

```
12-23 04:37:08.618 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: MainActivity TaskId: 26 hashCode: 1383099668
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:12.834 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: SingleTaskActivity TaskId: 26 hashCode: 1383477328
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:15.022 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTaskActivity TaskId: 26 hashCode: 1383477328
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:24.394 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: StandardActivity TaskId: 26 hashCode: 1383309812
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:31.562 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onCreate()方法**********
    onCreate: SingleTopActivity TaskId: 26 hashCode: 1383619516
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:32.834 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTopActivity TaskId: 26 hashCode: 1383619516
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:34.078 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTaskActivity TaskId: 26 hashCode: 1383477328
12-23 04:37:34.082 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:35.258 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTaskActivity TaskId: 26 hashCode: 1383477328
    taskAffinity: com.example.zhanghuabin.activitylunchmode
12-23 04:37:35.926 3039-3039/com.example.zhanghuabin.activitylunchmode I/info: **********onNewIntent()方法**********
    onNewIntent: SingleTaskActivity TaskId: 26 hashCode: 1383477328
    taskAffinity: com.example.zhanghuabin.activitylunchmode
```

我们看到使用 singleTask 启动模式启动一个 Activity，它还是在原来的 task 中启动。其实是这样的，我们并没有指定 taskAffinity 属性，这说明和默认值一样，也就是包名，当 MainActivity 启动时创建的 Task 的名字就是包名，因为 MainActivity 也没有指定 taskAffinity，而当我们启动 SingleTaskActivity ，首先会寻找需要的任务栈是否存在，也就是 taskAffinity 指定的值，这里就是包名，发现存在，就不再创建新的 task，而是直接使用。当该 task 中存在该 Activity 实例时就会复用该实例，这就是栈内复用模式。 

singleTask 启动模式启动 Activity 时，首先会根据 taskAffinity 属性去寻找当前是否存在一个对应名字的任务栈：

* 如果不存在，则会创建一个新的 Task，并创建新的 Activity 实例入栈到新创建的 Task 中去
* 如果存在，则得到该任务栈，查找该任务栈中是否存在该 Activity 实例。如果存在实例，则将它上面的 Activity 实例都出栈，然后回调启动的 Activity 实例的 `onNewIntent()` 方法；如果不存在该实例，则新建 Activity，并入栈。 

此外，我们可以将两个不同 App 中的 Activity 设置为相同的 taskAffinity，这样虽然在不同的应用中，但是 Activity 会被分配到同一个 Task 中去。

# singleInstance-全局唯一模式

该模式具备 singleTask 模式的所有特性外，与它的区别就是，这种模式下的 Activity 会单独占用一个 Task 栈，具有全局唯一性，即`整个系统中就这么一个实例`，由于栈内复用的特性，后续的请求均不会创建新的 Activity 实例，除非这个特殊的任务栈被销毁了。以 singleInstance 模式启动的 Activity `在整个系统中是单例的`，如果在启动这样的 Activiyt 时，已经存在了一个实例，那么会把它所在的任务调度到前台，重用这个实例。

在多个应用中启用它：

本应用中：

```java
<activity android:name=".singleinstance.SingleInstanceActivity"
            android:launchMode="singleInstance">

            <intent-filter>
                <action android:name="com.example.zhanghuabin.activitylunchmode.singleinstance"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
 </activity>
```

其他应用中：

```java
Intent intent = new Intent();
intent.setAction("com.example.zhanghuabin.activitylunchmode.singleinstance");
startActivity(intent);
```

SingleInstance 模式启动的 Activity 在系统中具有全局唯一性。


https://github.com/huabinzhang427/ActivityLunchMode.git






