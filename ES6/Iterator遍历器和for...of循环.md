# Iterator和for...of循环
我们需要一种统一的接口机制，来处理"集合"的所有不同的数据结构（Array、Object、Map、Set）

遍历器（Iterator）它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterrator接口，就可以完成遍历操作

Iterator的作用
* 为各种数据结构，提供一个统一的、简便的访问接口
* 使得数据结构的成员能够按某种次序排列
* ES6创造了一种新的遍历命令  for...of 循环，Iterrator接口主要提供for...of消费。

具有Symbole.iterator属性的数据结构如下：
* Array
* Map
* Set
* String
* TypedArray
* 函数的arguments对象
* NodeList对象

对于原生部署Iterator接口的数据结构，不用自己写遍历器函数，for...of循环会自动遍历他们



## Iterator
>默认的Iterator接口部署在数据结构的Symbol.iterator属性上，活着说一个数据结构只要具有Symbol.iterator属性，就可以认为是"可遍历的"
Symbol.iterator属性本身上一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器


map就是通过获取数组的Symbol.iterator属性函数得到的遍历器。得到遍历器就可以执行next函数，done表示遍历是否结束。
```
{
	let arr = ['hello', 'world'];
	let map = arr[Symbol.iterator]();
	console.log(map.next());		//{value: "hello", done: false}
	console.log(map.next());		//{value: "world", done: false}
	console.log(map.next());		//{value: undefined, done: true}
}
```

* 自定义遍历器
对象没有遍历器接口，需要自己在Symbole.iterator属性上面部署，这样才会被for...of循环遍历。


```
{
	let obj = {
		start: [1, 3, 2],
		end: [7, 9, 8],
		[Symbol.iterator]() {
			let self = this;
			let index = 0;
			let arr = [...self.start, ...self.end];  //合并为一个数组
			let len = arr.length;
			return {
				next() {
					if (index < len) {
						return {
							value: arr[index++],
							done: false
						}
					} else {
						return {
							value: arr[index++],
							done: true
						}
					}
				}
			}
		}
	}

	for (let key of obj){
		console.log(key);
	}
	//1
	//3
	//2
	//7
	//9
	//8

}
```
