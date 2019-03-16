# Set和Map数据结构

* set类似于数组，但是成员是唯一的，没有重复值，可以认为是集合。
set和数组的区别就是，是否有重复值
* Map类似于对象，也是键值对的集合，但是"键"的访问不限于字符串，各种数据类型都可以当作键。


## set
* Set集合的创建

```
{
	let list = new Set();

	//add方法添加元素
	list.add(5);		//add方法添加集合值
	list.add(7);
	console.log(list.size);	//求集合元素个数，类似数组的length属性
}


{
	//初始化创建Set元素
	let arr = [1, 2, 3, 4, 5];
	let list = new Set(arr);
}
```

* 应用数组的去重
>注意去重时不会转义变量的数据类型

  ```
  {
  	//数组去重
  	let arr = [1, 2, 3, 1, '2'];
  	let list = new Set(arr);
  	console.log([...list]);			//[1, 2, 3, "2"]    1被去重了，但是2 和 '2'没有哦 ，因为去重时不会做类型转化
  }
  ```

* Set集合的操作

  * add 新增元素
  * delete 删除元素
  * clear 清空集合
  * has   判断是否包含某元素
  * size属性  获取集合的个数

```
{
	//集合操作
	let arr = ['add', 'delete', 'clear', 'has'];
	let list = new Set(arr);

	console.log('has', list.has('add'));	//has true	判断集合重是否存在某个元素
	console.log('add', list.add('tmp'));	//add   Set(5) {"add", "delete", "clear", "has", "tmp"}
	console.log('delete', list.delete('add'), list);	//delete true Set(4) {"delete", "clear", "has", "tmp"}
	console.log('size',list.size)			//size 4    集合的元素个数
	console.log('clear',list.clear(),list);  //clear undefined Set(0) {}
}
```

* 集合的遍历
keys(),values(),entries(),forEach()

```
{
	//集合的遍历
	let arr = ['add', 'delete', 'clear', 'has'];
	let list =new Set(arr);

	for (let [key, val] of list.entries()) {
		console.log(key,val);
	}
	//key 和value是一样的
	//add add
	//delete delete
	//clear clear
	//has has

	list.forEach(function (item) {
		console.log(item);
	})
	//add
	//delete
	//clear
	//has

	for (let key of list.keys()){
		console.log(key);
	}

	for (let value of list.values()){
		console.log(value);
	}
}
```


## WeakSet() 和 Set()区别：
>①：WeakSet()支持的元素只能对象，②：加入的对象是浅引用，只是拷贝的一个地址。

>不能遍历、没有clear方法、没有size属性， 其他的方法和set一样

```
{
	let weaklist = new WeakSet();

	let arg = {};

	weaklist.add(arg);
	console.log(weaklist);	//WeakSet {{…}}
}
```


## Map

* map数据类型的定义
```
{
	//Map的构造函数也接受一个数组作为参数，该数组的成员是一个个表示键值对的数组
	let map = new Map([
		['a', 123],
		['b', 456]
	]);
	console.log(map);		//Map(2) {"a" => 123, "b" => 456}

	//上面的写法实际执行的算法是
	let arr=[
		['a', 123],
		['b', 456]
	]
	arr.forEach(
		([key,val])=>map.set(key,val)
		);
	console.log(map);     //Map(2) {"a" => 123, "b" => 456}  注意Map和Set一样原则去重
}
```

* Map的操作和属性
  - set 添加元素
  - delete 删除元素
  - clear 清空元素
  - size   元素到个数

```
{
	//Map的操作
	let map = new Map([
		['a', 123],
		['b', 456]
	]);
	console.log(map);		//Map(2) {"a" => 123, "b" => 456}

	console.log(map.size);	//2    Map的元素个数
	console.log(map.set('c',789));		//Map(3) {"a" => 123, "b" => 456, "c" => 789}  添加元素
	console.log(map.delete('c'),map);	//true Map(2) {"a" => 123, "b" => 456}     删除元素
	console.log(map.clear(),map);	//undefined Map(0) {}   元素到清空
}
* 遍历和Set是一样的

## Weakmap
>和map的区别和 set 和WeakSet的区别是一样的  weakmap的key只能说对象

>不能遍历、没有clear方法、没有size属性， 其他的方法和map一样



## map  set  arr obj 的比较
结论：可以抛弃之前的obj和arr 该用 map 和set ，优先使用map ,当需要唯一性时考虑set结构


```
{
	//Map、Set和Array比较增 查 改 删
	let map = new Map();
	let arr = [];
	let set = new Set();

	//增
	map.set('t', 1);
	set.add({t: 1});
	arr.push({t: 1});


	console.info('map-arr', map, set, arr);			//map-arr  Set(1)[{t: 1}]  Map(1){"t" => 1} [{t: 1}]

	//查
	let map_exists = map.has('t');
	let set_exists = set.has({t: 1});
	let arr_exists = arr.find(item => item.t);

	console.info('map-arr', map_exists, set_exists, arr_exists);		//true  false {t: 1}

	//改
	map.set('t', 2);
	set.forEach(item => item.t ? item.t = 2 : '');
	arr.forEach(item => item.t ? item.t = 2 : '');
	console.info('map-arr', map, set, arr);			//map-arr Map(1) {"t" => 2}   Set(1)[{t: 2}]  [{t: 2}]

	//删
	map.delete('t');
	set.forEach(item => item.t ? set.delete(item) : '');
	let index = arr.findIndex(item => item.t);
	arr.slice(index, 1);
	console.info('map-arr', map, set, arr);			//map-arr Map(0) {}  Set(0) {} [{}]
}
```

```
{
	//Map、Set 和 Object 查 改 删
	let map = new Map();
	let set = new Set();
	let obj = {};
	let item = {t:1};

	//增
	map.set('t',1);
	set.add(item);
	obj['t']=1;

	console.info('map-set-obj',map,set,obj);    //map-set-obj   Map(1) {"t" => 1}     Set(1)[{t: 1}]   {t: 1}

	//查
	console.info({
		map_exists:map.has('t'),
		set_exists:set.has(item),
		obj_exists:'t' in obj
	})				//{map_exists: true, set_exists: true, obj_exists: true}

	//改
	map.set('t',2);
	item.t = 2;				//set内对象是浅拷贝，修改原对象，set内数据会发生变化
	obj['t'] = 2;
	console.info('map-set-obj',map,set,obj);    //map-set-obj   Map(1) {"t" => 2}     Set(1)[{t: 2}]   {t: 2}

	//删除
	map.delete('t');
	set.delete(item);
	delete obj['t'];
	console.info('map-set-obj',map,set,obj);	//map-set-obj Map(0) {} Set(0) {} {}
}
```
