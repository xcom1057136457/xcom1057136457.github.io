---
title: 关于DOM中事件的介绍
date: 2019-09-11 11:03:19
---
## 1.介绍（或者时DOM事件怎么使用和获取数据）

1. 获取事件源：使用dom选择器去选择你想要绑定事件的dom
2. 为事件绑定事件处理函数
   事件类型：click、focus、blur、mouseover、mouseout、....
   事件处理函数：当绑定的事件类型被触发的时候该函数执行
3. 从事件对象中获取事件详细信息
   当事件处理函数执行的时候，dom会将事件对象传递给事件处理函数event

## 2.事件流

> 前提条件：元素嵌套，每层元素上绑定事件

```
<div class="outer">
    <div class="center">
        <div class="inner">

        </div>
    </div>
</div>
```

**1.事件捕获顺序：外层 -> 内层（上方代码捕获顺序为：outer > center > inner）
2.事件冒泡：内层 -> 外层（上方代码冒泡顺序为：inner > center > outer）**

## 3.事件对象

属性或API|事件对象
:--:|:--:
target|目标对象，用户操作的那个dom对象
currentTarget|当前目标对象，表示事件处理函数绑定的那个dom对象
stopPropagation()|停止冒泡
preventDefault()|阻止默认行为
keyCode|按键值

## 4.事件类型

属性|当以下情况发生时，出现此事件
:--:|:--:
click|鼠标点击某个对象
dbclick|鼠标双击某个对象
keyup|键盘弹起时
keydown|鼠标按下时
keypress|鼠标完成一次操作时（鼠标按下弹起）
focus|元素获得焦点
blur|元素失去焦点
mouseup|某个鼠标按键被松开
mousedown|某个鼠标键被按下
mouseover|光标移动到元素上，支持子元素
mouseout|光标移出元素，支持子元素
mouseenter|光标移动到元素上，不支持子元素
mouseleave|光标移出元素，不支持子元素
onload|某个页面或图像被完成加载

## 5.事件代理

**原理：将事件绑定在当前元素的父元素上而非当前元素上，这时候当点击当前元素的时候，执行父元素上绑定的事件处理函数，可以通过event.target获取当前元素**

好处：父元素代理子元素所有的事件，子元素可以动态添加和删除而不用频繁绑定事件

## 6.事件绑定的方式

1. onxxx:属性，简单，兼容性比较好
2. addEventListenner、removeEventListener:方法，非IE低版本才能兼容
3. attachEvent、detachEvent：方法，IE低版本下才能兼容
