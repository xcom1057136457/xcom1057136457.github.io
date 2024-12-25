---
title: CSS课堂梳理
date: 2019-08-25 11:02:55
---
## 介绍

### 1.层叠样式

### 2.层叠

* 多个样式表修饰同一个元素
* 继承
* 优先级

### 3.样式表

``` css
{
    color:#fff;
}
```

----

## 在HTML中的用法

### 1.内嵌样式表：在元素中添加style属性，style属性值为CSS样式规则

`<div style="width: 100px; height: 100px;"></div>`

缺点：样式与结构杂糅
优点：简单直接，优先级高

### 2.内部样式法：将样式添加到head标签中的style标签里

``` css
<style>

</style>
```

### 3.外部样式法：将样式定义在CSS文件中，通过`<link rel="stylesheet" href="./CSS文件名.CSS">`连接

---

## 语法

```
选择器{
    样式名:样式值;
    样式名:样式值;
    ...
}
```
---

## 选择器

### 选择器的类别

1. 核心选择器
2. 层次选择器
3. 属性选择器
4. 伪类选择器
5. 伪元素

### 核心选择器

选择器|代码
:--:|:--:
标签选择器|div{}
id选择器|#one{}
class选择器|.second{}
逗号选择器|div,#one{}、ul,ol{}
组合选择器|div#one{}
普遍选择器|*

### 层次选择器

选择器|代码格式
:--:|:--:
子元素选择器|nav > ul > li{}
后代选择器|.nav li{}
下一代兄弟选择器|.products > li.ios +*{}
之后所有兄弟选择器|.products > li.ios ~*{}

### 属性选择器

**作用：过滤（表单元素）**

选择器|含义
:--:|:--:
selector{}|选择某个标签
input[placeholder]|选择在input中具有placeholder的元素
input[type=text]|选择在input中type为text的元素
input[type^=t]|选择以input中type的值以t开头的元素
input[type$=t]|选择以input中type的值以t结尾的元素
input[type*=t]|选择以input中type的值含有t的元素

### 伪类选择器

**作用：过滤器**

#### 1.与状态有关

选择器|含义
:--:|:--:
:link|a标签还未被访问
:hover|光标悬浮到元素上
:active|a标签激活
:visited|a标签访问过

#### 2.与子元素有关

选择器|含义
:--:|:--:
:first-child|选择第一个子元素
:last-child|选择最后一个子元素
:nth-child(v)|选择第v个子元素
:first-of-type|选择第一种类型的子元素
:last-of-type|选择最后一中类型的子元素
:ntc-of-type(n)|选择同类型中的第n个同级兄弟元素

### 伪元素

选择器|含义
:--:|:--:
::after|选择子元素最后的位置
::before|选择子元素最前面的位置
::first-letter|选择第一个字符
::first-line|选择第一段字符
::selection|选择被选中的文字
