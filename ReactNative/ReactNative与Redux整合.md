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

>当使用了React Navigation的项目中，想要集成redux就必须引入react-navigation-redux-helpers这个库

* 第一步：安装react-navigation-redux-helpers，这个包允许通过redux来管理react navigation的state

```
npm install --save react-navigation-redux-helpers
```

[github地址V3](https://github.com/react-navigation/react-navigation-redux-helpers)

* Example

[文档出处](https://github.com/react-navigation/redux-helpers)

```
import {
  createStackNavigator,
} from 'react-navigation';
import {
  createStore,
  applyMiddleware,
  combineReducers,
} from 'redux';
import {
  createReduxContainer,
  createReactNavigationReduxMiddleware,
  createNavigationReducer,
} from 'react-navigation-redux-helpers';
import { Provider, connect } from 'react-redux';
import React from 'react';

//创建导航器
const AppNavigator = createStackNavigator(AppRouteConfigs);

//创建导航器的reducer
const navReducer = createNavigationReducer(AppNavigator);

//将导航器的reducer合并到store的reducer
const appReducer = combineReducers({
  nav: navReducer,
  ...
});

//创建导航器的中间件，最终加入到store中
const middleware = createReactNavigationReduxMiddleware(
  state => state.nav,
);

//创建导航器的Redux容器
const App = createReduxContainer(AppNavigator);

//下面是demo如何从state中获取nav
const mapStateToProps = (state) => ({
  state: state.nav,
});
const AppWithNavigationState = connect(mapStateToProps)(App);

const store = createStore(
  appReducer,
  applyMiddleware(middleware),
);

class Root extends React.Component {
  render() {
    return (
      <Provider store={store}>
        <AppWithNavigationState />
      </Provider>
    );
  }
}
```

//关键点几个点：
1：createReactNavigationReduxMiddleware 创建中间件，加入到store的中间件中
2：createNavigationReducer  创建reducer，合并到store的reducer中
3：createReduxContainer  将导航器创建为组件


## 自定义中间件

注意写法
```
const logger = store => next => action => {
	if (typeof action === 'function') {
		console.log('dispatching a function');
	}else{
		console.log('dispatching ',action);
	}
	const result = next(action);
	console.log('nextState ',store.getState());
};


const middlewares = [
	middleware,
	logger
]
//创建store
export default createStore(
	 reducers,
	 applyMiddleware(...middlewares)
);
```


## 处理android中的物理返回键
[文档出处：](https://github.com/react-navigation/redux-helpers)

```
  componentDidMount() {
		BackHandler.addEventListener("hardwareBackPress", this.onBackPress);
	}

	componentWillUnmount() {
		BackHandler.removeEventListener("hardwareBackPress", this.onBackPress);
	}

	onBackPress = () => {
		const { dispatch, nav } = this.props;
		// alert(nav.routes[1].index)
		if (nav.routes[1].index === 0) {  //没有上一层路由没了  注意这个地方
			return false;
		}
		dispatch(NavigationActions.back());
		return true;
	};

  //获取nav
  const mapStateToProps = state=>({
  	nav:state.nav
  })

  export default connect(mapStateToProps)(HomePage)
```
