---
title: DOM的基本介绍及方法
date: 2019-09-02 14:52:01
---
> Dom是JS操作HTML的api

## 在HTML中插入相关代码

### 在HTML中添加CSS代码（复习）

1. 在HTML中的元素中加入style属性

2. 在HTML中加入style标签即`<style></style>`

3. 在head中引入相关CSS文件或者第三方CSS即`link href = "CSS文件名或链接.CSS"`

### 在HTML中添加js代码

1. 事件属性/href

2. 在HTML中加入script标签即`<script></script>`

3. 在HTML中引入相关script文件或者第三方script即`script src = "script文件名或链接.js"`

## DOM构造函数树

```
                                         Node(节点)
Doucument（文档）              Element（元素）       Text（文本）             Comment（注释）
HTMLDoucument（html文档）   HTMLElement（HTML元素）
```

**根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：**
   * 整个文档是一个文档节点
   * 每个 HTML 元素是元素节点
   * HTML 元素内的文本是文本节点
   * 每个 HTML 属性是属性节点
   * 注释是注释节点


> 请看下面的HTML片段

``` html
<html>
  <head>
    <title>DOM 教程</title>
  </head>
  <body>
    <h1>DOM 第一课</h1>
    <p>Hello world!</p>
  </body>
</html>
```

从上面的HTML中：

   * `<html>`节点没有父节点；它是根节点
   * `<head>`和`<body>`的父节点是`<html>`节点
   * 文本节点"Hello world!"的父节点是`<p>`节点

并且：
   
   * `<html>`节点拥有两个子节点：`<head>`和`<body>`
   * `<head>`节点拥有一个子节点：`<title>`节点
   * `<title>`节点也拥有一个子节点：文本节点"DOM教程"
   * `<h1`>和`<p>`节点是同胞节点，同时也是`<body>`的子节点

并且：
   * `<head>`元素是`<html>`元素的首个子节点
   * `<body>`元素是`<html>`元素的最后一个子节点
   * `<h1>`元素是`<body>`元素的首个子节点
   * `<p>`元素是`<body>`元素的最后一个子节点

## 第三方JS库【操作HTML】

### 1.实例化（如何创建对象）

`var one = document.getElementById("one");`

实例化不需要去实例document，浏览器已经自动实例化好了，直接用就行

### 2. API

#### 1.node

**html中所有的内容都可以认为是节点，比如：doctype、html、head、注释、div内容'hello'、空格、回车都是节点**

```
属性：
        1.获取节点基本信息的属性
                nodeType
                        ELEMENT_NODE       1
                        TEXT_NODE          3
                        COMMENT_NODE       8
                        DOCUMENT_NODE      9
                nodeName
                        document           #document
                        文本               #text
                        元素               标签名大写，如DIV
                nodeValue
                        文本，注释         内容

        2.表示层次结构的属性
                parentNode:父节点
                parentElement:父元素
                ownerElement:当前元素所在的文档对象
                childNodes:孩子节点【类数组】
                firstChild:第一个孩子节点
                lastchild:最后一个孩子节点
                nextSibling:下一个兄弟节点
                previousSibling:上一个兄弟节点
```

```
方法：
        1.父节点调用的方法
                appendChild():在父节点中添加一个新的子节点
                insertBefore(new,reference):在某个子节点上方插入一个新的子节点
                replaceChild(new,old):用新的子节点取代一个旧的子节点
        
        2.其他
                cloneNode([boolean]):克隆一个旧节点，如果参数为true，表示克隆
```

#### 2.Document

**文档，表示整个HTML文档或者xml文档，一般情况下一个HTML可以使用一个Document的实例来表示，即document**

```
属性
        body
        head
        title
```

```
方法
        getElementById()：通过某个元素id来获取相关元素
        getElementsByClassName()：通过某个元素Class来获取相关元素【一般为一个类数组】
        getElementsByTagName()：通过某个元素名称如div、p来获取相关元素【一般为一个类数组】
        getElementsByName()：通过某个元素的name来获取相关元素【一般为一个类数组】
```

#### 3.Element

**元素，HTML文档中所有的元素都可以映射为一个Element实例（如body/div/p/span.....）**

```
属性
        1.元素属性相关的属性
                id
                className
            自有属性
                name
                href
                src
                alt
                target
                ...
            自定义属性
        
        2.元素层次结构相关属性
                children:子代元素【一般为一个类数组】
                firstElementChild:第一个子元素
                lastElementchild:最后一个子元素
                nextElementSibling:下一个兄弟元素
                previousElementSibling:上一个兄弟元素
                innerHTML:获取或设置一个元素内的html内容
                innerText:获取或设置一个元素内的文本内容
```

```
方法
        getAttribute(key):获取某个元素的内容
        setAttribute(key):设置某个元素的内容
```

#### 4.Text

`<div>hello world</div>`
文本内容，如上'hello world'表示文本内容

#### 5.Comment

` <!--注释内容--> `
注释如上方
