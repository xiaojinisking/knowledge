# Reducer

## 概念
Reducers指定了应用状态的变化如何响应actions并将处理完的state发送到store。注意action只是描述了有事情发生了这一事实，并没有描述应用如何更新state.

Reducer是一个纯函数，接受旧的state和action,返回新的state

注意点：不要进行下列操作：
* 修改传入的参数
* 执行有副作用的操作，如：API请求和路由跳转

# 拆分Reducer

Redux 提供了combineReducers()工具来进行reder的合并

```
import {combineReducers} from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

等价于下面的：

```
export default function todoApp(state ={},action){
  return {
    visibilityFilter:visibilityFilter(state.visibilityFilter,action),
    todos:todos(state.todos,action)
  }
}
```

上面的visibilityFilter() 和todos() 分别是两个reducer函数


## 修改state数据

①：方法1：对象深拷贝

  const newState = JSON.parse(JSON.stringify(state));

  ②：方法2：Object.assign() 创建对象拷贝

  ```
  Object.assign(
        {},
        state,
        {visibilityFilter: action.filter}
      )
  ```
  ③：ES7的对象展开符
  ```
  return { ...state, visibilityFilter: action.filter }
  ```
最佳的解决方案是使用ES7的对象展开运算符
