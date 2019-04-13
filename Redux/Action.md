# Action

## Action的概念
Action是一个包含了 type字段的普通对象：

```
conset ADD_TODO = 'ADD_TODO';
{
  type:ADD_TODO,
  text:'Build my First Redux app'
}
```

多数情况下type被定义成字符串常量，单这个不是必须的，对于大项目来说是必须的，而且常量非常多了建议放入单独的文件

```
import {ADD_TODO,REMOVE_TODO} from '../actionTypes'
```

## Action创建函数
Action创建函数就是生成action的方法。注意区分action和action创建函数

```
function addTodo(txt){
  return {
    type:ADD_TODO,
    text
  }
}
```

dispath的时候可以通过如下来实现：
```
dispath(addTodo(text))

方法2：将上面的dispath 再装入一个 函数 ，然后直接调用还是来dispath
const boundAddTodo = text =>dispath(addTodo(text))

boundAddTodo(text) //直接调用
```
store 里面直接通过store.dispath()调用dispath()方法，但是多数情况下，我们使用react-redux提供的connect（）帮助来调用
