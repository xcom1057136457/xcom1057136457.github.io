---
title: CSS_布局
date: 2024-08-29 16:49:21
---
## 浮动布局

> float:left/right

**浮动布局会使浮动标签脱离文档流**

1. 宽度高度默认由内容决定
2. 原先所在位置就会被其他块元素抢占
3. 浮动元素在一行中依次排列，当一行无法容纳的时候会自动换行

### 应用

1. 全部浮动（超过两列）

``` css
ul>li{
	float:left;
	width:计算合适的宽度;
}

取消浮动(一般加在父元素上)
ul::after{
	display:block; //设置为块元素
	content:""; //将内容设置为空值
	clear:both; //清除周边元素
}
```

2. 左侧浮动，右侧不浮动（两列）

``` css
.content>.left{
	float:left;
	width:220px;
}

将另一个元素向右移230px
.content>.right{
	margin-left:230px;
}

<div class="content">
	<div class="left"></div>
	<div class="right"></div>
</div>
```

> 对于导航类的布局一般浮动li
该布局较难，多试验，多用就知道该如何使用

## 定位布局

### 作用 

**当一个元素悬挂在其他元素之上，优先考虑定位布局
eg: 模态框、下拉菜单、二级菜单、固定宣传栏、网页聊天页面**

### 用法

**position:static/relative/absolute/fixed**

#### static: 默认布局，默认文档流中，非定位元素 

#### relative: 定位元素(相对定位)

1. 没有脱离文档流
2. 参照点为当前元素原本位置

#### absolute: 定位元素(绝对定位) 

1. 脱离了文档流
2. 参照距离当前元素最近的父定位元素，如果所有的父元素都没有定位元素，参照浏览器视口

#### fixed: 定位元素(固定定位) 

1. 脱离了文档流
2. 参照浏览器视口

#### sticky: 定位元素(粘滞定位) 

1. 不脱离文档流
2. relative 和 fixed 的结合

### 特点

1. 可以使用描述当前元素位置的属性: top、right、bottom、left
2. 可以使用z-index(向上浮动多少)
3. 参照点(参照某个元素使用absolute、fixed、sticky)
4. 是否脱离文档流

## 伸缩盒布局

### 作用

**使得子元素在父元素中分列显示，与float的作用类似。一般用于响应式布局（手机app中）**

### 用法

1. 父元素在主轴上一定要有一个固定的宽/高
2. 子元素在交叉轴上默认宽/高占满父元素
3.
``` css
<ul>
	<li></li>
	<li></li>
	<li></li>
</ul>

ul{
	display:flex;
}
```

### ul伸缩盒

#### 1.设置父元素为伸缩盒(display: flex)

#### 2.主轴(flex-direction)

1. 主轴：默认情况下为x轴
2. 交叉轴默认情况下为y轴
3. 元素沿着伸缩盒的主轴排列的

#### 3.伸缩盒自动换行(flex-wrap)

1. nowrap: 默认值，不换行
2. wrap: 换行

### li伸缩盒的元素

1. 基础值: flex-basis(主轴上元素的基础值)
2. 对盈余空间的分配: flex-grow
3. 对亏损空间的贡献: flex-shrink

> 速写: flex: grow shrink basis;

## 例子

### 布局代码

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="./test2.css">
</head>
<body>
	<div class="container">
		<!-- 导航布局开始 -->
		<div class="wrapper">
			<div class="nav">
				<ul class="ul">
					<li>
						<a href="#">美食</a>
						<div>美食子元素</div>
					</li>
					<li>
						<a href="#">旅行</a>
						<div>旅行子元素</div>
					</li>
					<li>
						<a href="#">阅读</a>
						<div>阅读子元素</div>
					</li>
					<li>
						<a href="#">服饰</a>
						<div>服饰子元素</div>
					</li>
					<li>
						<a href="#">装饰</a>
						<div>装饰子元素</div>
					</li>
					<li>
						<a href="#">数码</a>
						<div>数码子元素</div>
					</li>
					<li>
						<a href="#">家电</a>
						<div>家电子元素</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
	<!-- 导航布局结束 -->

	<!-- 左fixed布局开始 -->
		<div class="left">
			<ul>
				<li>
					<a href="#">数码</a>
					<div>数码子元素</div>
				</li>

				<li>
					<a href="#">旅行</a>
					<div>旅行子元素</div>
				</li>

				<li>
					<a href="#">阅读</a>
					<div>阅读子元素</div>
				</li>

				<li>
					<a href="#">装饰</a>
					<div>装饰子元素</div>
				</li>

				<li>
					<a href="#">家电</a>
					<div>家电子元素</div>
				</li>
			</ul>
		</div>
		<!-- 左fixed布局结束 -->

		<!-- 右sticky布局开始 -->
		<div class="right">
			<ul>
				<li>
					<a href="#">数码</a>
					<div>数码子元素</div>
				</li>

				<li>
					<a href="#">旅行</a>
					<div>旅行子元素</div>
				</li>

				<li>
					<a href="#">阅读</a>
					<div>阅读子元素</div>
				</li>

				<li>
					<a href="#">装饰</a>
					<div>装饰子元素</div>
				</li>

				<li>
					<a href="#">家电</a>
					<div>家电子元素</div>
				</li>
			</ul>
		</div>
		<!-- 右sticky布局结束 -->
```

### 样式代码

``` css
/*导航样式开始*/
/*重置样式*/
*{
	padding: 0;
	margin: 0;
	list-style: none;
}

body{
	margin: 0;
	padding: 0;
}

ul{
	list-style: none;
	margin: 0;
	padding: 0;
}

.wrapper{
	margin: 0 auto;
	width: 1200px;
}

/*重置a的样式*/
.container > .wrapper > .nav > ul a{
	text-decoration-line: none;
	color: #333;
}

/*设置.container背景*/
.container{
	background-color: lightblue;
	line-height: 3em;
}

/*将ul设置为伸缩盒*/
.container .nav > ul{
	display: flex;
	position: relative;
}

/*将ul中的li分配空间*/
.container .nav > ul > li{
	flex-grow: 1;
	text-align: center;
}

/*将ul设置为相对布局*/
.container .ul{
	position: relative;
}

/*设置div的属性，隐藏*/
.container .ul > li > div{
	position: absolute;
	left: 0;
	top: 100%;
	width:100%;
	background-color: teal;
	height: 200px;
	line-height: 200px;
	display: none;
}

.container .ul > li:hover > div{
	display: block;
}
/*导航样式结束*/





/*左fixed样式开始*/

/*重置a标签样式*/
.left > ul > li a{
	text-decoration-line: none;
	color: #333;
}

/*将整个left.div设置为fixed布局*/
.left{
	position: fixed;
	left: 0;
	top: 200px;
	width: 100px;
	background-color: pink;
}

/*设置li的格式*/
.left > ul > li{
	height: 100px;
	line-height: 100px;
	border-bottom: 1px solid #fff;
	position: relative;
	text-align: center;
}

/*设置子元素格式*/
.left > ul > li > div{
	display: block;
	position: absolute;
	left: 100%;
	top: 0;
	width: 300px;
	background-color: teal;
	height: 300px;
	display: none;
}

/*将光标移动到li将div显示*/
.left > ul > li:hover > div{
	display: block;
}

/*左fixed样式结束 */




/*右sticky样式开始*/

/*重置a标签的样式*/
.right > ul > li a{
	text-decoration-line: none;
	color: #333;
}

/*将right设置为sticky布局，且浮动到右边*/
.right{
	position: sticky;
	float: right;
	width: 100px;
	background-color: pink;
	top: 200px;
}

/*将li设置为相对布局*/
.right > ul > li{
	text-align: center;
	height: 100px;
	line-height: 100px;
	border-bottom: 1px solid #fff;
	position: relative;
}

/*将div设置为绝对布局，且向右移动整个父元素的宽度*/
.right > ul > li > div{
	position: absolute;
	right: 100%;
	background-color: teal;
	width: 200px;
	top: 0;
	display: none;
}

/*将div显示*/
.right > ul > li:hover > div{
	display: block;
}



/*右sticky样式结束*/
```

### 成型模样

![image](https://i.loli.net/2019/08/05/dVSihRNfTlCeQA7.png)
