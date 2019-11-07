---
title: Android 启动保留一个Activity
date: 2019-11-07 19:58:48
tags:
---
## 情景
在APP中有三个Activity，分别为A、B和C，来回切换启动A、B、C，;在不销毁其他实例的情况，A、B、C保留一个实例。

对于这个问题，一般容易想到的是使用SingleTask模式，可是SingleTask有一个问题，会清空位于它栈上面其他实例。比如在一个栈存在A, B, C,三个实例，C启动A时，根据SingleTask模式，B,C会被清除掉；是否可以实现在不清除B，C的情况下，让A位于栈顶。可以通过一下设置来实现。
<!-- more -->
```kotlin
intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP)
intent.addFlags(Intent.FLAG_ACTIVITY_REORDER_TO_FRONT)
```
分别来看这两个标志位：
- FLAG_ACTIVITY_SINGLE_TOP
- FLAG_ACTIVITY_REORDER_TO_FRONT

#### FLAG_ACTIVITY_SINGLE_TOP
```
    /**
     * If set, the activity will not be launched if it is already running
     * at the top of the history stack.
     */
```
如果activity位于栈顶，则不会启动新的activity，设置这个标志位，当位于栈顶，启动相同activity不会创建新的activity。会调用onNewIntent()方法

#### FLAG_ACTIVITY_REORDER_TO_FRONT
```
    /**
     * If set in an Intent passed to {@link Context#startActivity Context.startActivity()},
     * this flag will cause the launched activity to be brought to the front of its
     * task's history stack if it is already running.
     *
     * <p>For example, consider a task consisting of four activities: A, B, C, D.
     * If D calls startActivity() with an Intent that resolves to the component
     * of activity B, then B will be brought to the front of the history stack,
     * with this resulting order:  A, C, D, B.
     *
     * This flag will be ignored if {@link #FLAG_ACTIVITY_CLEAR_TOP} is also
     * specified.
     */
```
当调用startActivity()方法启动时，如果已经运行并存在历史任务栈中，把改activity移动到栈顶

