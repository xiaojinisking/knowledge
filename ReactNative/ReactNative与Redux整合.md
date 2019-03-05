# 如何在ReactNative 中使用Redux

## 准备工作
根据需要安装如下组件：
* redux(必选)
* redux-redux(必选)：redux作者为方便在react上使用redux开发的一个用在redux上的redux库
* redux-devtools(可选)：Redux开发者工具支持热加载、action重放、自定义UI等功能
* redux-thunk:实现action异步的middleware
* redux-persist(可选)：支持store本地持久化
* redux-obsevable(可选)：实现可取消的action

```
yarn add redux
yarn add react-redux
yarn add redux-devtools
```

[react-redux文档](http://cn.redux.js.org/docs/react-redux/)


## React Native中使用了react-navigation 后如何与redux组合使用

* 第一步：安装react-navigation-redux-helpers，这个包允许通过redux来管理react navigation的state

```
npm install --save react-navigation-redux-helpers
```

[github地址](https://github.com/react-navigation/react-navigation-redux-helpers)
