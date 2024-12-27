---
title: CSS_动画
date: 2024-08-29 16:49:21
---
## 动画的定义

1.

``` css
@keyframes 动画名称{
	from{}
	to{}
}
```

> 这种一般用来设置中间动画无变化的情况，比如滑动。

2. 

``` css
@keyframes 动画名称{
	10%{}
	20%{}
	....
	100%{}
}
```

> 这种一般用来设置中间动画有变化的情况，比如球体大小的缩放。

## 动画的应用

### animation-name:动画名称

### animation-duration:动画的持续时间 + s

### animation-dalay:动画延迟 + s

### animation-direction:动画运动方向

* reverse: 动画效果从后往前运动，例：从from到to
* alternate：往返运动

### animation-fill-mode:  动画结束后保留哪个样式

* forwards: 保留最后一帧的样式
* backwards: 保留第一帧的样式

### animation-iteration-count:动画执行的次数

* 次数
* infinite（无限次）

### animation-timing-function:动画执行的时间曲线

* linear: 平滑的运行速率
* steps(): 直接跨越式运行，括号内的数字就是把运动的数值分成几部分，然后跨越运动，一般用来图片的运动

### animation-play-state:  动画播放状态

* running: 开始使用动画效果
* paused: 停止使用动画效果

### animation有速写模式，但是不要求记住

## 例子

**可以使用下面的代码通过改变属性值进行测试理解**

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
		*{
			margin: 0;
			padding: 0;
			list-style: none;
		}

		.container{
			height: 100px;
		}

		.ball{
			height: 100px;
			width: 100px;
			border-radius: 50%;
			background-color: teal;

			animation-name: x;
			animation-duration: 5s;
			animation-timing-function: linear;
			animation-iteration-count: infinite;
			animation-direction: alternate;
		}

		@keyframes x{
			from{
				margin-left: 0px;
			}

			to{
				margin-left: 1200px;
			}
		}
	</style>
</head>
<body>
	<div class="container">
		<div class="ball"></div>
	</div>
</body>
</html>
```

## 代码运行成品

![image](https://b2.bmp.ovh/imgs/2019/08/3ac01143099e015e.png)

