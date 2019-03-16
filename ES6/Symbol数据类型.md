# Symbol数据类型

## 什么事Symbol
symbol是一种新的数据类型，表示独一无二的值。
通过Symbol()函数来生成Symbol数据


```
{
	let s1 = Symbol();
	let s2 = Symbol();

	console.log(s1 === s2);		//false

	let s3 = Symbol('foo');
	let s4 = Symbol('foo');

	console.log(s3 === s4);		//false

	console.log(s3.toString());	//Symbol(foo)

	console.log(s3.toString() === s4.toString());	//true   是两个字符串"Symbol(foo)"   Symbol函数加参数就是为了做标识的。toString就可以看到

  let s5 = Symbol.for('foo');
	let s6 = Symbol.for('foo');	//Symbol.for()，先去全局检查是否存在使用foo标记生产单Symbol值，如果有就用之前的Symbol值，否则就生产一个新的Symbol值，并注册到全局
	console.log(s5===s6);			//true
}
```

##用途
* 作为属性名
```
{
	let obj = {
		[Symbol()]:'jack',
		'age':20
	}

	console.log(obj);		//{age: 20, Symbol(): "jack"}

	let a1 = Symbol('abc');
	let obj2 = {
		[a1] : 345,
		c:456,
		'abc':123
	}

	console.log(obj2);		//{c: 456, abc: 123, Symbol(abc): 345}
}
```
