---
title: jQuery的基本介绍及用法
date: 2019-09-14 12:38:00
---
> 复习

1. iconfont提供了图标
2. moment提供了Date的api
3. lodash封装了数组的api
4. jQuery封装了DOM、BOM、ECMAScript

> DOM问题(jQuery的出现可以完美解决dom中的问题)

1. 获取元素比较复杂，querySelector【兼容性较差】
2. dom操作较为复杂，不能进行批量操作

## 引用jQuery

**jQuery = $**

**在script直接使用$即可代表jQuery**

## 使用jQuery

用法|描述
:--:|:--:
$(function(){})|当文档加载完毕后回调函数执行
$("`<div>`one`</div>`")|创建dom元素，并且将其转换为jquery对象
$("选择器")|通过选择器找到满足要求的dom对象，并且封装到jquery对象中
$(dom对象)|将一个dom对象转换为jquery对象

## jQuery对象和dom对象区别

**jquery更像NodeList,是一个类数组对象，但是比NodeList厉害多了，jquery构造函数原型中有超级多的方法**

## 特点

**1.兼容性好
2.简化dom操作
3.链式操作（dom操作方法返回值为当前jQuery对象）
4.不污染顶级变量 $/jQuery**

## jQuery对象

**jQuery对象，类数组对象。数组中的元素是DOM对象**   
<br>

`var one = document.getElementById("one")`
**one是Element的实例，可以调用Element、Node**   

`var $one = $(one)`
**$one是jQuery的实例，可以调用jQuery**

## 属性、css

### attr():获取或者设置attribute（核心、自有、自定义）
参数：
**1.attr(key)
2.attr(key,val)**

### prop():获取或者设置property（核心、自有）
参数：
**1.prop(key)
2.prop(key,val)**

### removeAttr(attrName)：移除attribute

### removeProp(propName)：移除property

### val()：获取或设表单元素的值

### css()：获取或者设置样式
参数：
**1.css(key)
2.css(key,val)**

### addClass():为每个匹配的元素添加指定的类名

### removeClass()：为每个匹配的元素移除指定的类名

### hasClass()：检查当前的元素是否含有某个特定的类，如果有，则返回true

## DOM操作

API|操作
:--:|:--:
clone()|克隆jQuery对象
wrap()|为选中元素添加一个父元素
append()|为选中元素追加一个子元素
prepend()|为选中元素插入一个子元素
after()|在当前元素后添加一个兄弟元素
before()|在当前元素前添加一个兄弟元素
empty()|清空子元素
remove()|移除当前元素
replace()|替换元素
html()|获取或设置一个元素的html内容
text()|获取或试着一个元素的text内容

## 遍历

API|操作
:--:|:--:
each(function(index,item){})|以每一个匹配的元素作为上下文来执行一个函数
map(function(index,item){})|将一个数组中的元素转换到另一个数组中
eq()|获取类数组中的某一个jQuery对象
filter(expr|obj|ele|fn)|筛选出与指定表达式匹配的元素集合
first()|获取第一个元素
last()|获取最后一个元素
not(selector)|从匹配元素的集合中删除与指定表达式匹配的元素
parent([`selector`])|取得一个包含着所有匹配元素的唯一父元素的元素集合
parents(selector)|取得一个包含着所有匹配元素的祖先元素的元素集合（不包含根元素）。可以通过一个可选的表达式进行筛选
children([`selector`])|取得一个包含匹配的元素集合中每一个元素的所有子元素的元素集合
find(selector)|搜索所有与指定表达式匹配的元素。这个函数是找出正在处理的元素的后代元素的好方法
next()|取得一个包含匹配的元素集合中每一个元素紧邻的后面同辈元素的元素集合
prev()|取得一个包含匹配的元素集合中每一个元素紧邻的前一个同辈元素的元素集合
siblings()|取得一个包含匹配的元素集合中每一个元素的所有唯一同辈元素的元素集合。可以用可选的表达式进行筛选

## 事件机制

### on(eventType[,代理],handler)

### off(eventType[,代理],handler)

### click(handler);

### mouseover()

### mouseout()

### focus()

### blur()
