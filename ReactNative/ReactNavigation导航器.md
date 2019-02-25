# React Navigation 导航器

[官方文档](https://reactnavigation.org/docs/zh-Hans/getting-started.html)

[API文档](https://reactnavigation.org/docs/zh-Hans/api-reference.html)

[简介](http://www.devio.org/2018/05/15/navigator-to-react-navigation/)


## 安装React Navigation

### 在项目中安装react-navigation 包

>yarn add react-navigation

然后安装第三方扩展 react-native-gusture-handler.处理顶部手势
>yarn add react-native-gesture-handler

Link所有都原生依赖，因为react-native-gesture-handler关联到了原生应用

>react-native link react-native-gesture-handler

![](assets/markdown-img-paste-20190223170038865.png)

>当使用到原生的时候，还需要配置些，查看官方手册说明


## React Navigation 的分类

* stack navigator 为应用提供了一种在屏幕之间切换并管理导航历史记录的方式。 web浏览器和React Navigation 工作原理的一个主要区别是：React Navigation 的stack navigator 提供了再Android和Ios设备上，在堆栈中的路由之间导航时你期望的手势和动画。
