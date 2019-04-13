# 组件的状态 state
因为每个组件之后根据props渲染自己一次，props是不可变的，为了实现交互，可以使用组的state.

this.state是私有的，可以通过getInitialState()方法初始化，通过调用this.setState()来改变它，组件就会重新渲染自己。render()方法依赖于this.props和this.state，框架会确保渲染出来的UI界面总是与输入的this.props和this.state保持一致。


## 初始化state
* 通过getInitialState()方法，在组件的生命周期中仅执行一次

```
getInitialState:function(){
  return {favorite:false}
}
```
* 活着在组件类的construct方法内进行定义

```
construct(props){
  super(props);
  this.state={
    title:''
  }
}
```

## 更新state

通过this.setState()方法来更新state,调用该方法后，React会重新渲染相关的UI。

使用setState()需要注意下面几点：
* 1：状态的更新可能是异步的

React可以将多个setState()调用合并成一个来调用提供性能。

因为this.props和this.state可能是异步更新到，你就不应该依靠他们都值来计算下一个状态。

错误的写法
```
this.setState({
  counter :this.state.counter + this.props.increment
})
```


解决方案：setState使用函数的形式,这个函数可以接受两个参数，第一个参数表示上一个状态值，第二个参数表示当前的props
```
this.setState((prevState,props)=>{
  counter : prevState.counter + props.increment
  })
```
