# React Native 列表组件

## 列表分类

* FlatList  高性能简单列表组件
* SwipeableFlatList  是FlatList的升级，带有测滑功能
* SectionList 带有分组/类/区 功能的列表


## FlatList
* [文档地址：](https://reactnative.cn/docs/flatlist/)

支持功能：
* 完全跨平台。
* 支持水平布局模式。
* 行组件显示或隐藏时可配置回调事件。
* 支持单独的头部组件。
* 支持单独的尾部组件。
* 支持自定义行间分隔线。
* 支持下拉刷新。
* 支持上拉加载。
* 支持跳转到指定行（ScrollToIndex）。


* demo

```
import React, {Component} from 'react';
import {
	Platform,
	StyleSheet,
	Text,
	View,
	Button,
	FlatList,
	RefreshControl,
	ActivityIndicator
} from 'react-native';

type Props = {};
const CITY_NAMES=['北京','上海','广州','深圳','杭州','苏州','成都','武汉','郑州','洛阳','厦门','青岛','拉萨'];
export default class FlatListDemo extends Component<Props> {
	constructor(props){
		super(props);
		this.state = {
			isLoading:false,
			dataArray:CITY_NAMES
		}
	}

	loadData(refreshing){
		if(refreshing){
			this.setState({
				isLoading:true
			});
		}
		setTimeout(()=>{
			let dataArray = [];
			if(refreshing){
        //数据转为倒序
				for(let i = this.state.dataArray.length-1;i>=0;i--){
					dataArray.push(this.state.dataArray[i]);
				}
			}else{
				// dataArray  = this.state.dataArray.concat(CITY_NAMES)
				dataArray  = [...this.state.dataArray, ...CITY_NAMES]
			}
			this.setState({
				// dataArray:[...this.state.dataArray,...dataArray],
				dataArray:dataArray,
				isLoading:false
			})
		},2000)
	}

	_renderItem(data){
		return (
			<View style={styles.item}>
				<Text style={styles.text}>{data.item}</Text>
			</View>
		)
	}

	genIndicator(){
		return <View style={styles.indicatorContainer}>
			<ActivityIndicator
				style={styles.indicator}
				size={'large'}
				color={'red'}
				animating={true}
			/>
			<Text>正在加载更多</Text>
		</View>
	}


  render() {
    return (
      <View style={styles.container}>
				<FlatList
					data={this.state.dataArray}
					renderItem={(data)=>this._renderItem(data)}
					keyExtractor={(item,index) => (index)}
					// refreshing={this.state.isLoading}
					// onRefresh={()=>{
					// 	this.loadData()
					// }}
					refreshControl={	//单独定制头部组件
						<RefreshControl
							title={'Loading'}
							colors={['#f0f']}
							titleColor={'red'}
							tintColor={'orange'}
							refreshing={this.state.isLoading}
							onRefresh={()=>{
								this.loadData(true)
							}}
						/>
					}

					ListFooterComponent={()=>this.genIndicator()}	//下拉加载效果
					onEndReached={()=>{
						this.loadData();
					}}
				/>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,

    backgroundColor: '#F5FCFF',
  },
  item:{
  	backgroundColor: '#169',
		height:200,
		marginRight:15,
		marginLeft:15,
		marginBottom:15,
		alignItems: 'center',
		justifyContent: 'center'
	},
	text:{
		color: 'white',
		fontSize: 20
	},
	indicatorContainer:{
  	alignItems: 'center'
	},
	indicator:{
  	color: 'red',
		margin:10
	}
});


```

![](https://github.com/xiaojinisking/knowledge/blob/master/assets/screencast-Genymotion-2019-03-03_16.25.32.958.gif?raw=true)



## SwipeableFlatList
带侧滑菜单

[一些参考博客](https://www.jianshu.com/p/68f9d59bf437)


demo

```
import React, {Component} from 'react';
import {
	Platform,
	StyleSheet,
	Text,
	View,
	Button,
	FlatList,
	SwipeableFlatList,
	RefreshControl,
	ActivityIndicator,
	TouchableHighlight
} from 'react-native';

type Props = {};
const CITY_NAMES=['北京','上海','广州','深圳','杭州','苏州','成都','武汉','郑州','洛阳','厦门','青岛','拉萨'];
export default class SwipeableFlatListDemo extends Component<Props> {
	constructor(props){
		super(props);
		this.state = {
			isLoading:false,
			dataArray:CITY_NAMES
		}
	}

	loadData(refreshing){
		if(refreshing){
			this.setState({
				isLoading:true
			});
		}
		setTimeout(()=>{
			let dataArray = [];
			if(refreshing){
				for(let i = this.state.dataArray.length-1;i>=0;i--){
					dataArray.push(this.state.dataArray[i]);
				}
			}else{
				// dataArray  = this.state.dataArray.concat(CITY_NAMES)
				dataArray  = [...this.state.dataArray, ...CITY_NAMES]
			}
			this.setState({
				// dataArray:[...this.state.dataArray,...dataArray],
				dataArray:dataArray,
				isLoading:false
			})
		},2000)
	}

	_renderItem(data){
		return (
			<View style={styles.item}>
				<Text style={styles.text}>{data.item}</Text>
			</View>
		)
	}

	genIndicator(){
		return <View style={styles.indicatorContainer}>
			<ActivityIndicator
				style={styles.indicator}
				size={'large'}
				color={'red'}
				animating={true}
			/>
			<Text>正在加载更多</Text>
		</View>
	}

	genQuickAction(){
		return <View style={styles.quickContainer}>
			<TouchableHighlight
				onPress={()=>{
					alert('确认删除?')
				}}
			>
				<View style={styles.quick}>
					<Text style={styles.text}>删除</Text>
				</View>
			</TouchableHighlight>
		</View>
	}


  render() {
    return (
      <View style={styles.container}>
				<SwipeableFlatList
					data={this.state.dataArray}
					renderItem={(data)=>this._renderItem(data)}
					keyExtractor={(item,index) => (index)}
					// refreshing={this.state.isLoading}
					// onRefresh={()=>{
					// 	this.loadData()
					// }}
					refreshControl={	//自定义上拉刷选
						<RefreshControl
							title={'Loading'}
							colors={['#f0f']}
							titleColor={'red'}
							tintColor={'orange'}
							refreshing={this.state.isLoading}
							onRefresh={()=>{
								this.loadData(true)
							}}
						/>
					}

					ListFooterComponent={()=>this.genIndicator()}	//下拉加载效果
					onEndReached={()=>{//底部加载数据
						this.loadData();
					}}

					renderQuickActions={() => this.genQuickAction()}//侧滑菜单
					maxSwipeDistance={100}//可展开（滑动）的距离
					// bounceFirstRowOnMount={false}//进去的时候不展示侧滑效果
				/>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,

    backgroundColor: '#F5FCFF',
  },
  item:{
  	backgroundColor: '#169',
		height:100,
		marginRight:15,
		marginLeft:15,
		marginBottom:15,
		alignItems: 'center',
		justifyContent: 'center'
	},
	text:{
		color: 'white',
		fontSize: 20
	},
	indicatorContainer:{
  	alignItems: 'center'
	},
	indicator:{
  	color: 'red',
		margin:10
	},
	quickContainer:{
  	flex:1,
		flexDirection:'row',
		justifyContent: 'flex-end',
		marginRight: 15,
		marginBottom: 15
	},
	quick:{
  	backgroundColor:'red',
		flex:1,
		alignItems:'flex-end',
		justifyContent:'center',
		padding:10,
		width:200
	}
});

```
![](https://github.com/xiaojinisking/knowledge/blob/master/assets/screencast-Genymotion-2019-03-03_16.59.33.025.gif?raw=true)

## SectionList 带有分组/类/区 功能的列表
[文档地址](https://reactnative.cn/docs/sectionlist/)

* demo

```
import React, {Component} from 'react';
import {
	Platform,
	StyleSheet,
	Text,
	View,
	Button,
	SectionList,
	RefreshControl,
	ActivityIndicator
} from 'react-native';

type Props = {};
const CITY_NAMES=[{data:['北京','上海','广州','深圳'],title:'一线'},{data:['杭州','苏州','成都','武汉','郑州'],title:'二线'},{data:['洛阳','厦门','青岛','拉萨'],'title':'三线城市'}];
export default class SectionListDemo extends Component<Props> {
	constructor(props){
		super(props);
		this.state = {
			isLoading:false,
			dataArray:CITY_NAMES
		}
	}

	loadData(refreshing){
		if(refreshing){
			this.setState({
				isLoading:true
			});
		}
		setTimeout(()=>{
			let dataArray = [];
			if(refreshing){
				for(let i = this.state.dataArray.length-1;i>=0;i--){
					dataArray.push(this.state.dataArray[i]);
				}
			}else{
				// dataArray  = this.state.dataArray.concat(CITY_NAMES)
				dataArray  = [...this.state.dataArray, ...CITY_NAMES]
			}
			this.setState({
				// dataArray:[...this.state.dataArray,...dataArray],
				dataArray:dataArray,
				isLoading:false
			})
		},2000)
	}

	_renderItem(data){
		return (
			<View style={styles.item}>
				<Text style={styles.text}>{data.item}</Text>
			</View>
		)
	}

	genIndicator(){
		return <View style={styles.indicatorContainer}>
			<ActivityIndicator
				style={styles.indicator}
				size={'large'}
				color={'red'}
				animating={true}
			/>
			<Text>正在加载更多</Text>
		</View>
	}

	_renderSectionHeader({section}){
		return <View style={styles.sectionHeader}>
			<Text>{section.title}</Text>
		</View>
	}

  render() {
    return (
      <View style={styles.container}>
				<SectionList
					sections={this.state.dataArray}
					renderItem={(data)=>this._renderItem(data)}
					keyExtractor={(item,index) => (index)}
					// refreshing={this.state.isLoading}
					// onRefresh={()=>{
					// 	this.loadData()
					// }}
					refreshControl={	//自定义上拉刷选
						<RefreshControl
							title={'Loading'}
							colors={['#f0f']}
							titleColor={'red'}
							tintColor={'orange'}
							refreshing={this.state.isLoading}
							onRefresh={()=>{
								this.loadData(true)
							}}
						/>
					}

					ListFooterComponent={()=>this.genIndicator()}	//下拉加载效果
					onEndReached={()=>{
						this.loadData();
					}}
					renderSectionHeader={(data)=>this._renderSectionHeader(data)}
					ItemSeparatorComponent={()=><View style={styles.separator}/>}	//item之间的分割线
				/>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,

    backgroundColor: '#fafafa',
  },
  item:{
		height:80,
		// marginRight:15,
		// marginLeft:15,
		marginBottom:15,
		alignItems: 'center',
		justifyContent: 'center'
	},
	text:{
		color: 'black',
		fontSize: 20
	},
	indicatorContainer:{
  	alignItems: 'center'
	},
	indicator:{
  	color: 'red',
		margin:10
	},
	sectionHeader:{
  	height:50,
		backgroundColor:'#93ebbe',
		alignItems:'center',
		justifyContent: 'center'
	},
	separator:{
		height:1,
		backgroundColor:'gray',
		flex:1
	}
});

```

![](https://github.com/xiaojinisking/knowledge/blob/master/assets/screencast-Genymotion-2019-03-03_17.00.35.056.gif?raw=true)
