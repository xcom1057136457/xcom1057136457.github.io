---
title: JavaScript的一些基本介绍及应用
date: 2019-08-26 09:56:34
---
## Javascript组成

### 1.ECMAScript5 （Javascript语法标准）
**JS标准语法**

### 2.DOM  (document object model)
**JS操作HTML**

### 3.BOM（browser object model）
**JS操作浏览器**

## JS解释器

### 1.在所有的主流浏览器中都具备JS解释器

### 2.JS运行在浏览器端：动画、表单验证、Ajax数据交互

### 3.JS运行在服务器端：JS转换、代码编译、操作数据库、流、网络、IOT

## 变量

### 强类型

**变量的数据类型取决于变量的声明**

### 弱类型

**变量的数据类型取决于值的类型**

## 变量类型

### 基本数据类型

数据类型|变量
:--:|:--:
数字类型|number
字符串类型|string
布尔类型|boolean
空对象|null object
未定义|undefined
a是什么类型|typeof a
如果result是NaN，那么这个函数返回true|isNaN(result)
如果result是一个有穷数，返回true|isFinite(result)

### 引用数据类型

**数组、函数、对象、正则表达式**

### 基本数据类型与引用数据类型在内存中的表示

#### 基本

&emsp; var a = "terry";

#### 引用

&emsp; var b = {
&emsp;&emsp;    name:"terry",
&emsp;&emsp;    age:12,
&emsp;&emsp;    gender:"male"
&emsp;&emsp;}

#### 基本数据类型的值保存在栈区;引用数据类型的引用地址保存在栈区，内容保存在堆区

## 操作符

### 算术操作符

进行的操作|算数操作符
:--:|:--:
+ | +=
- | -=
* | *=
/ | /=
% | %=

### 赋值操作符

**var result = 1 + 2;
result += 3;
==等价于==>
result = result + 3;**

### 一元操作符

操作 | 一元操作符
:--:|:--:
自增 | ++
自减 | --

**前置：先自增、减再参与其他运算
后置：先参与其他运算，再自增、自减**


### 逻辑运算符

操作 | 逻辑运算符
:--:|:--:
当第一个表达式为假的时候，不再计算第二个表达式，整个表达式为false | &&
当第一个表达式为真的时候，整个表达式的结果为true，不再计算第二个 | 11
将表达式的结果颠倒，而且具有将其他数据类型转换为boolean的作用 | ！

### 比较运算符（比较栈区的值）【返回类型为Boolean】

> 比较对象的时候比较的是对象的引用

==
！=

> 先比较数据类型，如果数据类型不一致，会直接返回false，如果数据类型一致，再比较值

===
!==

### 三目运算符

exp1 ? exp2 : exp3

**当exp1为真，返回exp2，否则返回exp3**

### 位运算符(number，先将number转换二进制再运算)

操作 | 位运算符
:--:|:--:
异或 | ^
位与 | &
位或 | 1

### 拼接运算符

**当使用“+”，操作符中出现了字符串，那么肯定是拼接运算**

## 类型转换

### 其他数据类型转换为Boolean

#### !!v
#### Boolean(v)

### 其他数据类型转换为String

#### v + ""
#### String(v);
#### v.toString();

### 其他数据转换为number

#### +v
#### -(-v)
#### Number();

## 流程控制语句

### 1.分支语句

1.


```
if(条件){
    进行的操作
}
```

2.


```
if(条件){
    进行的操作
}else{
    不是if那个条件，又该运行什么操作
}
```

3.

```
if(){
    
}else if(){

}else{

}
```

4.

```
switch(所要判断的值){
    case 某个条件:{
        操作
    }
    ....

    default:{
        case的条件都没有所进行的操作
    }
}
```

### 2.循环语句

> 三要素：初始化条件，结束判定条件，迭代

#### 1.for循环语句

&emsp; for(初始化条件;结束判定条件;迭代){
&emsp;&emsp;循环体
&emsp;}

#### 2.前置判断循环while循环

&emsp;初始化条件
&emsp;while(结束判定条件){
&emsp;&emsp;循环体
&emsp;&emsp;迭代
&emsp;}

#### 3.后置判断循环do-while循环

&emsp;初始化条件
&emsp;do{
&emsp;&emsp;循环体
&emsp;&emsp;迭代
&emsp;}while(结束判定条件);

## 对象

### 1.介绍
**复杂的数据类型，引用数据类型，一般情况下，对象中包含了多个属性和方法**

### 2.对象创建方式

#### 1.对象字面量

**对象使用"{}"作为边界，对象是由多个属性（方法是一种特殊的属性）来组成，每个属性之间通过","分割，属性名与属性值之间通过":"分割。属性名可以不使用双引号，当属性名中包含特殊字符一定要使用双引号，属性值一般为常量或者具体的值，也可以变量**

#### 2.使用构造函数创建对象

**var obj = new Object();**

### 3.属性的访问：

#### 点访问符
**对象名.属性名**   

#### 中括号访问符
**对象名["属性名"]**

### 4.所有的对象直接或者间接的继承Object，也就是说所有的对象都可以调用Object原型中的方法和属性

> Object.prototype.xxx
#### constructor:构造函数，谁创建了当前的对象
#### hasOwnProperty(prop):判断某个属性是否属于当前对象的自有属性
#### propertyIsEnumerable(prop):检测某个属性是否可以被枚举
#### isPrototypeOf(prop):检测某个属性是否是原型链中的属性
#### toString():返回该对象的字符串信息
#### valueOf():返回该对象的数字描述

### 5.删除属性

**delete obj1.name**

### 6.对象序列化和反序列化

#### 将js对象转换为json字符串

&emsp;**var json = JSON.stringify(obj)**

#### 将JSON字符串转换为js对象

&emsp;**var obj = JSON.parse(json)**

### 7.对象的遍历

```
var obj = {
    name:"terry",
    age:12;
}
var arr = ["terry","larry"];

for(var key in obj){
    //如何获取属性名
    var value = obj[key];
}

obj可以为对象或者数组;key表示对象的属性名或者是数组的索引;在运行的时候，每次
从obj中获取一个属性名或者索引赋值给key，然后执行循环体，循环.....
```

### 8.in关键字

**prop in obj
检测prop是否可以被obj调用**

## 函数（方法）

### 1.函数的用处

#### 1.使用函数封装某些功能代码，执行特定功能
#### 2.使用封装创建对象的模板【构造函数】面向对象

### 2.函数的使用（功能）

> 2.1函数的定义

#### 1）函数声明

```
funcftion 函数名(形参){
    函数体
}
```

#### 2) 函数表达式

```
var 函数名 = function(形参){
    函数体
}
```

> 2.2函数的调用

#### 函数名(实参)
#### 函数名.call(this,实参列表);
#### 函数名.apply(this,实参数组);

### 函数声明会提升

**如果一个函数使用函数声明的方式来定义，那么再函数定义之前就可以调用该函数**

### 函数的作用域

**如果一个变量声明再函数中，那么这个变量只能在函数中访问，当函数执行完毕后，这个变量就会被释放掉**

### 函数内部属性

> 只能在函数运行的时候才能确定的属性，只能在函数内部访问。

#### 1.形参
#### 2.arguments（接受实参的真正所在，类数组对象）
#### 3.this
&emsp;**如何判断this的值为谁：**
1. 如果函数使用"()"来调用，那看一下括号前面是不是函数名，如果是，看函数名前面有没有对象，如果有，this指向该对象，否则，指向全局对象(window/global)
2. 如果通过call,apply来调用,this为用户手动指向的那个对象

### 值传递和引用传递

```
var a = 3;
var b = a;   //b为3     值传递

b ++;
console.log(a);

var a = {name:"terry",age:12};
var b = a;      //a为指针，b为指针      引用传递

b.age ++
console.log(a.age);
```

## 数组

### 作用

**存放多个数据的集合**

### 定义方式

#### 1.数组字面量

```
var name = "terry";
var arr = [name,1,true,"hello",null,{name:"a"},function(){}];
```
#### 2.构造函数 Array

```
Array继承Object
通过Array构造函数构建出来的对象可以调用Array原型中的方法，还可以调用Object原型中的方法

var arr = new Array();
var arr = new Array(length);
var arr = new Array(item1,item2,......);

arr -> Array.prototype -> Object.prototype
```

### 数组访问方式

**中括号访问符，索引从0开始，如果指定索引的位置没有值，返回undefined**

### 数组内存

```
var arr = ["terry",12,[1,2,3]];

arr是变量，保存在栈区
arr是引用数据类型的变量，栈区保存的引用地址，数组的值保存在堆区
```

### 数组的属性

**数组也是一种对象，length是表示数组长度的属性，length表示数组中元素的个数**

### 数组的遍历(循环遍历索引)

`var arr = [1,2,3,4,5];`

#### 1.使用for循环

```
for(var i = 0; i < arr.length; i ++){
    var val = arr[i];
    console.log(val);
}
```

#### 2.使用while

#### 3.使用do-while

#### 4.使用增强for循环

```
for(var key in arr){
    var val = arr[key];
}
```

### 数组相关的API

#### 1.添加元素移除元素相关【改变原值】

1. push(p1,p2,....);
   **入栈，在数组的最后添加一个元素**
   参数：要入栈的元素
   返回值：数组长度
2. pop();
   **出栈，从数组的最后去除一个元素**
   参数：none
   返回值：出栈的元素
3. shift();
   **出队，将数组中第一个元素取出来**
   参数：none
   返回值：出队的元素
4. unshift(p1,p2,....);
   **插队，将元素插入在数组的最前面**
   参数：要插队的元素
   返回值：插队后队列的长度

#### 2.排序方法【改变原值】

1. sort()
   按照字符在字符编码表中出现的位置进行排序
2. sort(conparator)
   comparator为一个比较器函数，函数可以接受两个值a,b; 当a位于b之前的时候返回

#### 3.序列化方法

1. toString():将数组转换为字符串，数组中的元素通过逗号连接
2. join(v):将数组转换为字符串，数组中的元素通过v连接
3. JSON.stringify():将数组转换为JSON字符串

#### 4.截取方式

1. concat()【不改变原值】
   **将参数中的数组和当前数组合并为一个数组**
   参数：多个数组
   返回值：合并后的数组
2. slice(begin,end)【不改变原值】
   **从当前数组中截取一个子数组并取范围**
   参数：begin &emsp;&emsp;&emsp; 起始位置；end &emsp;&emsp;&emsp; 结束位置
   返回值：截取到的子数组
3. splice(begin,delete[p1,p2,p3...])【改变原值】
   **从数组中删除、插入、更新元素**
   参数：
   &emsp;&emsp;begin：起始位置（删除，插入）
   &emsp;&emsp;delete：删除的个数
   &emsp;&emsp;p1,p2,....：插入的值 
   返回值：删除的元素组成的数组

#### 5.迭代方法

1. forEach(function(item,index,arr){})
   **遍历当前数组**
   参数：function(item,index,arr){}
   &emsp;&emsp;每次遍历一次，这个匿名函数就会被调用一次，forEach将当前遍历的元素，索引，当前数组当做实参传递这个匿名函数
2. every(function(item,index,arr){})
   **判断数组中所有的元素是否满足回调函数中给定的条件**
   参数:function(item,index,arr){}
   &emsp;&emsp;当每次回调函数返回值为true，every方法的结果就为true，
   &emsp;&emsp;当回调函数返回值为false，every方法的结果就为false
3. some(function(item,index,arr){})
   **判断数组中是否有满足条件的元素**
   参数：function(item,index,arr){}
   &emsp;&emsp;当回调函数返回true，some方法的结果就为true
   &emsp;&emsp;当每次回调函数返回false，some方法的结果才为false
4. filter(function(item,index,arr){})
   参数：function(item,index,arr){}
   &emsp;&emsp;当回调函数返回true，当前元素就会被添加到返回值数组中
   返回值：数组
5. map(function(item,index,arr){})
   参数：function(item,index,arr){}
   &emsp;&emsp;回调函数可以返回任意类型的值，这些值都会被添加到map方法的返回值数组中
   返回值：数组
   
### 6.查找方法

**indexOf()**
**lastIndexOf()**

## 包装器类型

### Number

### Boolean

### String

**String.prototype.xxx**

函数|描述
:--:|:--:
length|获取字符串中字数的子量
charAt(index)|获取指定索引处的字符
charCodeAt(index)|获取指定索引处的字符编码
indexOf()|获取指定字符在字符串中的索引
lastIndexOf()|获取指定字符在字符串中的所有，从后往前
concat()|连接两个字符串
slice(begin,end)|截取子字符串，begin开始位置，end结束位置，不包含结束位置
substring（begin，end)|截取子字符串，begin开始位置，end结束位置，不包含结束位置
substr(begin,length)|截取子字符串，begin开始位置，length截取的字符的个数
trim()|删除字符串左右两边的空格
toUpperCase()|转换为大写
toLowerCase()|转换为小写

### RegExp

**RegExp.prototype.xxx**

#### search(RegExp)

&emsp;&emsp;测试，类似于test(),不支持global
&emsp;&emsp;返回值匹配内容的索引

#### match(RegExp)

&emsp;&emsp;查找匹配的内容
&emsp;&emsp;返回值数组，保存了匹配的内容


## Math对象

### Math.min()：返回最小值

### Math.max：返回最大值

### Math.random():获取0~1之间的随机小数

### Math.round():四舍五入

### Math.ceil():向上舍入

### Math.floor():向下舍入

## 正则表达式

### 1.实例化正则表达式对象

#### 1.构造函数模式
   &emsp;var pattern = new RegExp("正则表达式","模式");
   &emsp;var pattern = new RegExp("[0-9]{4}-[0-9]{7}","g");

#### 2.正则表达式字面量
   &emsp;var pattern = /正则表达式/模式
   &emsp;var pattern = /[0-9]{4}-[0-9]{7}/gim

### 2.正则表达式

1. 字符类

    字符|描述
    :--:|:--:
    .|匹配任何单个字符
    \d|匹配任意数字，等价于[0-9]
    [0-9]|匹配中扩展任意一个字
    \D|匹配任意非数字
    [^0-9]|等价于\D，上箭头在中括号中表示非
    ^[0-9]|表示任意一个数字作为一行的开始
    \w|字符，等价于[a-zA-Z0-9_]
    \W|非字符，等价于[^a-ZA-Z0-9]
    \s|空白字符，tab,return,space
    \S|非空白字符

2. 分组

   **/[a-z][0-9]/ig:匹配一个字符后拼接一个数字**
   **/([a-z])([0-9])/ig:匹配一个字符后拼接一个数字，分组匹配字符，分组匹配数字**

3. 数量词

    数量词|描述
    :--:|:--:
    exp(3)|将前面的表达式匹配三次
    exp(3,5)|将前面的表达式匹配3~5次
    exp(3,)|将前面的表达式匹配3+次
    exp*|将前面的表达式匹配0次或多次
    exp+|将前面的表达式匹配1次或多次
    exp?|将前面的表达式匹配0次或一次

> 贪婪匹配（默认情况下）: /\d{2,}/【尽可能多的匹配】

> 非贪婪匹配：/\d{2,}?/【尽可能少的匹配】

### 3.模式

**i：ignoreCase
g：global
m：multiline**

### 4.引用

属性|描述
:--:|:--:
lastIndex|如果模式中包含g,每次执行test或者exec的时候，都会维护这个属性，下一次搜索开始的位置;如果模式中不包含g,lastIndex为0
ignoreCase|如果模式中包含i，返回true
global|如果模式中包含g，返回true
multiline|如果模式中包含m，返回true
source|正则表达式的字符串表示
flags|正则表达式模式的字符串表示

方法|描述
:--:|:--:
test(str)|使用正则表达式对象测试str，如果str满足正则表达式返回true,否则返回false
exec(str)|从str中查找出满足正则表达式的字符串

