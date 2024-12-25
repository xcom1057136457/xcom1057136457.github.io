---
title: CSS_过渡效果
date: 2019-08-25 14:14:07
---
## transition和animation的区别

1. transition必须要触发，一般使用hover
2. transition不需要设置关键帧

> 简单的过度效果使用transition, 复杂的动画使用animation

## 用法

### transition-property：指定过渡属性

* 可以指定一个属性
* 可以指定多个属性
* 可以指定所有的属性（all）

### transition-duration：过渡持续的时间

**可以指定秒、以及毫秒**

### transition-timing-function: 过渡的时间曲线，一般使用linear(线性曲线，平滑过渡)

* linear: 规定以相同速度开始至结束的过渡效果
* ease: 规定慢速开始，然后变快，然后慢速结束的过渡效果
* ease-in: 规定以慢速开始的过渡效果
* ease-out: 规定以慢速结束的过渡效果
* ease-in-out: 规定以慢速开始和结束的过渡效果

### transition-delay: 过渡延迟

**可以指定秒、以及毫秒**

### 速写：transition:property duration timing delay;

## 关于transform

### scale（图片变大的倍数）

### skew（角度大小+deg）

### rotate（角度大小+deg）

### translate（往坐标轴前进的距离+px）

## 举例代码

``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Transform</title>
	
	<style>
		/*重置*/
		*{
			margin: 0;
			padding: 0;
			list-style: none;
		}
		
		/*加宽度，高度自适应，让盒子居中，剪切溢出来的图片部分，并把图片带来的空格取消*/
		.box {
			width: 500px;
			margin: 50px auto;
			overflow: hidden;
			border: 5px solid lightblue;
			font-size: 0;
		}
		
		/*让图片铺满div，并加过渡效果*/
		.box > img{
			width: 100%;
			transition-property: transform;
			transition-duration: 2s;
			transition-timing-function: linear;
		}
		
		/*放大的过渡效果，可以在这里测试过渡的各种效果*/
		.box:hover > img{
			transform: scale(1.3);

		}
	</style>

</head>
<body>
	<div class="box">
		<img src="./images/cat.jpg" alt="">
	</div>
</body>
</html>
```

## 代码图片以及效果

![image](https://i.loli.net/2019/08/08/kOfjC4DY7ibPewv.jpg)

![image](https://i.loli.net/2019/08/08/fgbOBHDaLyh9TXk.png)
