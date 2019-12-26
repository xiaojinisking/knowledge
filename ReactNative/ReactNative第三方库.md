# ReactNative 第三方库

# React Native 学习资源精选仓库
https://juejin.im/entry/59dd786cf265da431d3ba6e3

## 导航库 react-navigation
官网[https://reactnavigation.org/docs/zh-Hans/getting-started.html](https://reactnavigation.org/docs/zh-Hans/getting-started.html)

## react-navigation与redux的连接库
[v3](https://github.com/react-navigation/react-navigation-redux-helpers)

## 矢量图标

* [文档地址](https://github.com/oblador/react-native-vector-icons)
* [库查询地址](https://oblador.github.io/react-native-vector-icons/)
* [修改提示-Unrecognized font family ‘Ionicons’](https://blog.csdn.net/zhaolaoda2012/article/details/82627735)

基本使用

```
yarn add react-native-vector-icons
react-native link react-native-vector-icons

ios的需要cd ios  然后执行 pod install
```

引入不同类型的Icon库
```
import Ionicons from 'react-native-vector-icons/Ionicons';
import  MaterialIcons from 'react-native-vector-icons/MaterialIcons'

使用不通的Icon
<Ionicons
					name={'ios-home'}
					size={26}
					style={{color:tintColor}}
					/>

<MaterialIcons
					name={'drafts'}
					size={24}
					style={{color:tintColor}}
			/>

```

![](assets/markdown-img-paste-20190228144205110.png)


## 用于显示HTML标签的组件 React-native-htmlview





## 轮播 react-native-swiper
[github](https://github.com/leecade/react-native-swiper)
[android上报错ViewPagerAndroid修复](https://blog.csdn.net/weixin_43762903/article/details/102794751) 或升级到最新开发版
