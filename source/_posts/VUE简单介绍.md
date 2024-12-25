---
title: VUE简单介绍
date: 2019-10-17 10:01:49
---
## 1. 介绍

> VUE是一套用于构建用户界面的渐进式框架

**特点：**

1. 使用es6的语法进行vue的编程
2. 虚拟dom操作
3. 声明式渲染，响应式（零DOM操作）
4. 使用指令来完成条件渲染，列表渲染
5. 双向数据绑定
6. 声明式的事件绑定

## 2. VUE实例对象

``` javascript
let vm = new Vue({
    el,
    data:{message : "hello"},
    methods
})
```
**通过Vue的实例对象vm可以直接访问data中声明的变量以及methods中声明的方法**
**在methods的方法中，this指向当前Vue实例对象也就是vm**

## 3. 生命周期

### 1. beforeCreate
Vue实例创建完成，但没有初始化完成

### 2. created
vue实例初始化完成，此时可以通过vue的实例对象访问data中的变量以及methods中的方法

### 3. beforeMount
完成数据绑定之前的准备工作，重点完成dom对象的编译，将dom对象绑定在this.$el上，data中的变量还未绑定在dom对象

### 4. mounted
data中的变量绑定到了dom对象，并且dom渲染到了网页中

### 5. beforeUpdate
data中的变量的值发生了变化

### 6. updated
data中的变量的值的改变已经反映到了网页上

### 7. beforeDestroy
当前vue实例在销毁之前

### 8. destroyed
当前vue实例已经销毁



##  4. 模板语法

### 1. 双大括号
``` vue
{{ message }} //常量
{{ 1+1 }} //计算
{{ foo() }} //函数
```

### 2. 指令

``` vue
v-html
v-on
v-bind
v-if v-else-if v-else
v-show
v-for
```



## 5. 事件机制

### 1. 事件绑定

``` vue
v-on:eventType
<div v-on:click="handler"></div>
<div v-on:click="js表达式"></div>
```

### 2. v-on缩写

``` vue
<div @click="handler"></div>
<div @click="js表达式"></div>
```

### 3. 参数

``` vue
<div @click="handler('a')"></div>
<div @click="handler(a)"></div>
<div @click="handler(表达式)"></div>
```

### 4. 事件对象

```
1. 事件处理函数中没有参数
<div @click="handler"></div>

methods:{
  handler(event){
	event就是事件对象
  }
}

2. 事件处理函数中有参数
<div @click="handler(1,$event)"></div>

methods:{
  handler(a,event){
	a为1
	event就是事件对象
  }
}
```

### 5.修饰符

```
1) 事件修饰符
	eventType.prevent		阻止事件的默认行为
	eventType.stop 			停止事件的冒泡
	eventType.once 			仅绑定一次事件
	eventType.self 			仅绑定一次事件
	eventType.capture 	在事件捕获阶段执行事件处理函数
2) 按键修饰符
	支持keypress、keyup、keydown
	.enter .tab
	.delete (捕获“删除”和“退格”键)
	.esc .space .up .down .left .right
3) 系统修饰键
	.ctrl .alt .shift .meta
	@keypress.ctrl.enter
	主要应用在快捷操作上，例如ctrl+c ctrl+x
```

## 6. 属性绑定

```
<input type="text" v-bind:value="常量">
<input type="text" :value="变量">
<input type="text" :value="表达式">
```

## 7. style与class的绑定

```
1) class
   <div class="current"></div>
   <div v-bind:class="current"></div>
   <div v-bind:class="对象"></div>
   <li v-for="item in list"
	   :class="{basic:true,current:item.id === currentTab}" 
	   :key="item.id"
	   @click="changeTab(item.id)"
	>
	    {{item.name}}
	</li>
	<div v-bind:class="数组"></div>
		<li v-for="item in list"
			:class="['basic',foo(item.id)]" 
			:key="item.id"
			@click="changeTab(item.id)"
		>
			{{item.name}}
		</li>
		
2) style
   <div style="background: pink;color: #fff">hello world</div>
   <div :style="{background:'pink',color:'#fff'}">hello world</div>
   <div :style="[{background:'pink'},aaa]">hello world</div>

```

## 8. 深入组件

### 1. 组件定义

```
将组件的信息配置到一个对象中，这个对象与Vue的参数基本一致
let config = {
	template:``,
	data(){			//保证每个vue实例都具有一个唯一的data对象
		return {

		}
	},
	props:["title"]	,	//期望从调用者哪里获取到的属性名称
	methods:{}
}
```

### 2. 组件注册

> 全局注册：所有的vue实例都可以调用注册组件

```
Vue.component(组件名称,config)
```

> 局部注册：只有当前vue实例才能调用注册的组件

```
new Vue({
	el:"#app",
	components:{
		组件名称:config
	}
})
```

> 动态组件

```
App.vue 根组件
<component is="currentPage"></component>

import Customer from './pages/Customer'
	export default {
		data:{
			currentPage:"Customer"
		},
		components:{
			Customer
		}
}
```



### 3. 组件调用

```
<组件名称></组件名称>
```

### 4. 父组件向子组件传值（属性作为参数进行传递）

```
props:["title","type"]

参数类型校验
props:{
	a:Number,
	b:{		// 期望b的类型为字符串，并且这个参数是必须的
		type:String,
		required:true	
	},
	c:{		// 期望c的类型为boolean，如果这个参数用户没有传递，默认值为true  
		type:Boolean,
		default:true
	},
	d:Function
}

静态传参值为字符串，如果想要传递非字符串类型的值，那么必须使用动态传参

单向数据流: 父组件中data值的改变会影响到子组件中的相应数据的改变，但是子组件无法改变父组件的值
```

### 5. 事件传递 

```
当用户操作子组件的时候，希望父组件的值得到改变
父组件
	<my-test @foo="handler"></my-test>
子组件
{
	template:`<button @click="change">按钮</button>`,
	methods:{
		change(){
			this.$emit("foo")
		}
	}
}
流程：点击按钮-> change() ->this.$emit("foo") ->handler()
```

### 6. 插槽（模板作为参数传递）

> 匿名插槽

```
<my-test>
	<div>hello world</div>
</my-test>

let my-test = {
	template:`
		<div>
			<h2>{{title}}</h2>
			<div class="content">
			<!--slot用于接受组件调用时传递的子内容-->
				<slot></slot>
			</div>
		</div>
	`
}
```

> 具名插槽

```
<!--vue2.0-->
<my-test>
	<template v-slot:a>
		<div>hello world</div>
	</template>
	<template v-slot:b>
		<div>address...</div>
	</template>
</my-test>

<!--vue1.0-->
<my-test>
	<div slot="a">hello world</div>
	<div slot="b">address...</div>
</my-test>

let my-test = {
	template:`
		<div>
			<h2>{{title}}</h2>
			<div class="content">
				<slot name="a"></slot>
			</div>
			<div class="footer">
				<slot name="b"></slot>
			</div>
		</div>
		`
}
```

> 回调插槽（作用域插槽）

```
function foo(a,b,c,callback){
	console.log(a);
	console.log(b);
	c.forEach(function(item){
		callback(item)
	})
}	

foo(
	1,
	2,
	[
		{a:1,b:"terry",c:12},
		{a:2,b:"larry",c:23}
	],
	function(item){
		console.log(item.a);
		console.log(item.b);
		console.log(item.c);
	}
)

foo(
	3,
	4,
	[
		{id:1,name:"terry"},
		{id:2,name:"larry"}
	],
	function(item){
		console.log(item.id);
		console.log(item.name);
	}
)
```

## 9. 脚手架 vue-cli

### 1. 作用

> 创建并且初始化一个项目

```
mkdir app01
cd app01
npm init -y
npm install vue --save
npm install babel-preset-es2015 --save-dev
项目结构
	.git
	node_modules
	build
	src
		aseets 			静态文件，图片
		components	功能组件 my-alert
		pages 			页面组件 Customer/Category
		App.vue
	main.js
	package.json

```

> 自动测试

**脚手架会提供一个静态服务器，自动对源码进行构建，然后部署到服务器上，这些操作不需要开发者操作，是自动完成的**

> 自动打包

```
.vue 	-> html/css/js
```

### 2. 如何应用 @vue/cli

> 安装nodejs

```
linux 	解压/opt ;配置环境变量
windows 直接安装即可（无脑安装）
```

> 全局安装cnpm

```
> npm install -g cnpm --registry=https://registry.npm.taobao.org
> cnpm install yarn -g
```

> 全局安装 @vue/cli

```
> npm install @vue/cli -g
或
> cnpm install @vue/cli -g
或者
> yarn global add @vue/cli

测试：
> vue --version
```

> 创建项目

```
> vue create app01
> cd app01
// 安装依赖
> cnpm install axios qs --save
> vue add element
```

> 启动服务进行测试

```
> npm run served
```

## 10. 计算属性

> 与data类似，模板中可以直接访问计算属性中定义的属性

```
<p> {{name1}}</p>
<p> {{name2()}}</p>

new Vue({
	computed:{
		name1(){
			return xx;
		}
	},
	methods:{
		name2(){
			return xx;
		}
	}
})
```

> 计算属性相对于方法来说，计算属性会缓存计算后的值，而不是每次调用都会执行计算属性对应的函数。当计算属性中的因变量发生变化的时候，计算属性的值会重新计算

```
<template>
	<button @click="gender ='male' ">男</button>
	<button @click="gender ='female' ">女</button>
	<ul>
		<li v-for="item in studentsFilter">
			{{item}}
		</li>
	</ul>
</template>

new Vue({
	el:"#app",
	data:{
		students:[
			{id:1,name:"terry",gender:"male"},
			{id:1,name:"larry",gender:"male"},
			{id:1,name:"vicky",gender:"female"},
			{id:1,name:"lisa",gender:"female"},
			{id:1,name:"tom",gender:"male"},
		],
			gender:"male"
		},
		computed:{
			studentsFilter(){
				return this.students.filter(s => s.gender===this.gender)
			}
		}
})
当this.students或者this.gender发生变化的时候，计算属性会重新计算产生新的值，然后渲染到页面中
```

## 11. 监听器

> 用于监听data中值的变化，如果data中的值为对象，想要监听对象中属性的改变，那么必须deep:true

```
new Vue({
	el:"#app",
	data:{
		params:{}
	},
	watch:{
		"params":{
			handler:function(){
				console.log("==="+Math.random());
			},
			deep:true
		}
	}
})
```

## 12. vuex

### 1. 介绍

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

### 2. 核心概念

```
state 		状态 （data）
getters 	获取器(computed)
mutation 	突变（修改data值的唯一方式！）
action 		动作(封装异步代码，然后提交突变，进而修改state值)
```

### 3. 使用方式

> 实例化

```
let store = new Vuex.Store({
	state:{
		customers:[]
	},
	getters:{},
	mutations:{
		refreshCustomers(state,customers){}
	},
	actions:{}
})
```

> 集成到vue中

```
new Vue({
	el:'',
	data:{},
	store
})
```

> 调用

```
this.$store
```

### 4. mutations

> 作用：

突变，用于修改state的唯一方式，并且其中只能包含同步操作。

> 定义

```
mutations:{
	refresh(state,customers){
		state由vuex提供，customers是程序在提交mutation传递的参数
	}
}
```

> 调用

```
不能直接调用，必须通过store.commit方法来调用
store.commit(mutationName,payload)
payload 被称为载荷，实际就是传递给mutation的实参
```

> 辅助函数

```
Vuex.mapMutations(["findAll","deleteById"])
或
Vuex.mapMutations({
	findAllCustomer:"findAll"
}])
```

### 5. actions

> 作用：

动作，用于封装异步操作（异步操作的同步化），不能直接修改state,但是可以通过提交突变，间接修改state的值

> 定义:

```
actions:{
	async findAllCustomers(context,id){
	// context是vuex提供的，与store对象类似，可以直接访问commit、dispatch、getters、state,这个我们可以使用解构形式获取	context中的属性。id为该action第一个形参（载荷）
		let response = axios.get("");
		return response;
	}
}
```

> 映射辅助函数

```
Vuex.mapActions(["",""])
Vuex.mapActions({
	xxx:"xxx"
})
```

### 6. getters

> 作用

需要对state中的值经过一个计算，然后渲染到页面中的时候可以使用getters

> 定义

1. 使用属性调用

   ```
   getters:{
   	customerSize(state){
   		return state.customers.length
   	}
   }
   this.$store.getters.customerSize
   ```

2. 使用方法调用

   ```
   getters :{
   	customerFilter(state){
   		return (param){
   			return xxx
   		}
   	}
   }
   this.$store.getters.customerFilter(xx)
   ```

> 辅助函数

```
Vuex.mapGetters([""])
```

## 13. 路由机制 vueRouter

> 安装

```
cnpm install vue-router --save
```

> 实例化vueRouter实例对象

```
import Home from './pages/Home.vue'
let router = new VueRouter({
routes:[{
			path:"/",
			name:"home",
			component:Home
		},{
			path:"/category",
			component:Category
		},{
			path:"/custoemr",
			component:Custoemr
		}]
	})
```

> 集成到vue中

```
Vue.use(VueRouter)

new Vue({
	store,
	router
})
```

> 使用

```
将组件渲染到哪里？
	一般渲染到根组件的router-view标签
	当路由改变，找到对应的组件，将该组件设置到router-view

App.vue 
<div id="app">
	<div class="header"></div>
	<div class="nav"></div>
	<div class="content">
		<router-view></router-view>
	</div>
</div>

router-link可以渲染为一个超链接，用户点击后修改浏url地址
```

> 编程式导航

```
router.push("/")
=>
router.push({path:'/'})
=>
router.push({name:"home"})

传递参数
router.push({path:"/",params:{},query:{}})
```

