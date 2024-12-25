---
title: HTML高级应用-Canvas画布
date: 2019-09-20 15:32:57
---
>  目标：熟悉绘制图形和图像

## Canvas属性：width、height



## Canvas配套API



### 获取Canvas元素

`var MyCanvas = document.getElementById('MyCanvas');`

### 获取Canvas元素操作上下文getContext对象

`var c = MyCanvas.getContext('2d');`



## 用实例学习Canvas的基本使用



### 开始绘制：beginPath();



### 结束绘制/闭合路径：closePath();



### 清除画布：clearRect(x,y,width,height);



#### 1.  绘制线段

```
涉及属性：
	strokeStyle: 线条颜色
	linewidth: 线条粗细

设计方法：
	moveTo(x,y)
	lineTo(x,y)
	stroke()
	
举例
	var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");
    
	mc.beginPath();
    mc.strokeStyle = "black";
    mc.lineWidth = 3;
    mc.moveTo(3,670);
    mc.lineTo(300,670);
    mc.stroke();
```



#### 2.   绘制矩形

```
涉及属性：
	fillStyle

涉及方法：
	fillRect(x,y,width,height)	//实心矩阵
	strokeRect(x,y,width,height)	//空心矩阵
	fill();
	
举例
	var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");
    
	 //矩形的绘制
    mc.beginPath();
    mc.fillStyle="yellow";
    mc.strokeRect(50,100,100,50);//空心矩形 设置x,y,w,h
    mc.fillRect(50,150,100,50);//实心矩形 设置x,y,w,h
```

#### 3.  绘制圆形

```
涉及方法：
	arc(x,y,r,beginAngle,endAngle,acticlockwise)
		圆心，半径，弧度，顺逆时针
		255,255,0*Math.PI/180,180*Math.PI/180
	stroke()	//空心圆
	fill()		//实心圆
	
举例
	var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");
	
	实心圆
    mc.beginPath();
    mc.strokeStyle = "black"
    mc.fillstyle = "black";
    mc.lineWidth = 3;
    mc.arc(350,350,200,90*Math.PI/180,270*Math.PI/180,false);
    mc.fill();
```

#### 4.  绘制文字

```
涉及属性：
	font("style(italic/bold),size,type")
	context.font = "50px Arial"
	
涉及方法：
	strokeText(str,x,y)		//空心字
	fillText(str.x.y)		//实心字
	
举例：
    var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");

	mc.beginPath();
    mc.font = "24px 微软雅黑";
    var x = 0;

    function pwd(){
        x ++;
        if(x > 500){
            x = 0;
        }
        mc.clearRect(0,630,700,100);
        mc.fillText("蒋俊是个大傻逼",x,650);
    }
    setInterval("pwd()",10);
```

#### 5.图像翻转

```
设计方法：
	save();		//产生一个与原图相同的一个异次元空间
	translate(x,y);		//设置旋转点
	rotate(angle);		//设置弧度
	restore();		//把旋转后的异次元空间映入原图
	
举例：
    var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");

	mc.beginPath();
    mc.strokeStyle = "black";
    mc.lineWidth = 3;
    mc.moveTo(3,670);
    mc.lineTo(300,670);
    mc.stroke();

    mc.save();
    mc.translate(3,670);
    mc.rotate(-90*Math.PI/180);

    mc.beginPath();
    mc.strokeStyle = "black";
    mc.lineWidth = 3;
    mc.moveTo(0,0);
    mc.lineTo(297,0);
    mc.stroke();

    mc.restore();
```

#### 6.  绘制渐变图形

```
涉及方法：
	var color = mc.createLinearGradient(0,0,0,500)		//创建渐变
	color.addColorStop(0,"blue");
	color.addColorStop(1,"yellow");		//追加颜色
	mc.fillStyle = color;
	mc.fillRect(0,0,500,500);
	
举例：
	var myCanvas = document.getElementById('myCanvas');
    var mc = myCanvas.getContext("2d");
    
	//绘制一个渐变矩形，首先需要去预设渐变效果
    var color = mc.createLinearGradient(0,0,500,0);//设置渐变方向
    color.addColorStop(0,"blue");//给我们的渐变添加颜色
    color.addColorStop(0.2,"red");//给我们的渐变添加颜色
    color.addColorStop(0.6,"yellow");//给我们的渐变添加颜色
    color.addColorStop(1,"green");//给我们的渐变添加颜色

    mc.fillStyle = color;
    mc.fillRect(50,300,300,150);//实心矩形 设置x,y,w,h

```

#### 7.  Canvas存储

```
设计属性：
	window.location=mc.toDataURL("image/png").replace("image/png","image/octet-stream");
```

