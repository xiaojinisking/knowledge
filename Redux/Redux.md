# Redux

[手册文档](https://www.redux.org.cn/)

## redux简介

### 什么是Redux

![](assets/markdown-img-paste-20190305213608104.png)

工作流程：
* 1：用户操作View 发出Action, 发出方式就用到dispatch方法。
* 2：假如没有中间件，Store自动调用Reducer,并且传入两个参数（当前State和收到的Action）,Reducer会返回新的State。如果有中间件，Store会将当前State和收到的Action传递给Middleware，Middleware会调用reducer，然后返回新的State.
* 3:State一旦有变化，Store就会调用监听函数，通知所有都订阅者，来更新View.

到此为止，一次用户交互流程结束。可以看到，在整个流程中数据都是单向流动的。

### Redux和Flux都对比

![](assets/markdown-img-paste-20190428140215510.png)

### Redux的优点
##
![](assets/markdown-img-paste-20190428140150457.png)
# 为什么要用Redux?


![](assets/markdown-img-paste-20190428140344958.png)

### Redux的三个基本原则


![](assets/markdown-img-paste-20190428140426440.png)


### Redux 有哪几部分构成？


![](assets/markdown-img-paste-20190428140452505.png)

## react-redux介绍
视图层绑定的几个概念
* Provider
* connect()
* selector
* dispatch


![](assets/markdown-img-paste-20190305225751744.png)
