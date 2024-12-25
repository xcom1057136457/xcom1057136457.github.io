---
title: CSS的基本介绍及应用
date: 2019-08-25 10:54:44
---
## 介绍

### 层叠样式

### 层叠

* 多个样式表修饰同一个元素
* 继承
* 优先级

### 样式表

``` css
{
    color:#fff
}
```

## 在HTML中如何应用CSS

### 1.在元素中添加style属性，style属性值为CSS样式规则

`<div style="width:100px; height: 100px;"></div>`

缺点：样式与结构杂糅
优点：简单直接，优先级高

### 2.将样式添加到head标签中的style标签里

``` css
<style>

</style>
```

### 3.创建一个.CSS文件，然后在HTML文件中`<link rel "stylesheet" href = "./CSS文件名.css">`

## 语法

```
选择器{
    样式名:样式值；
    样式名:样式值;
    。。。
}
```
