---
title: Javascript数据类型的检测
date: 2021-03-05 16:07:28
---
![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/f163ff39ec8a49b7ad713cf8b1563c88%7Etplv-k3u1fbpfcp-watermark.image)

**作为 JavaScript 的入门级知识点，JS 数据类型在整个 JavaScript 的学习过程中其实尤为重要。因为在 JavaScript 编程中，我们经常会遇到边界数据类型条件判断问题，很多代码只有在某种特定的数据类型下，才能可靠地执行**

## 数据类型的基本概念

**数据类型包含undefined，null，Boolean，String，Number，Symbol，Biglnt，Object等八种，前 7 种类型为基础类型，最后 1 种（Object）为引用类型，也是需要重点关注的，因为它在日常工作中是使用得最频繁，也是需要关注最多技术细节的数据类型。 而引用数据类型（Object）又分为图上这几种常见的类型：Array - 数组对象、RegExp - 正则对象、Date - 日期对象、Math - 数学函数、Function - 函数对象。**

**在这里，重点了解下面两点，因为各种 JavaScript 的数据类型最后都会在初始化之后放在不同的内存中，因此上面的数据类型大致可以分成两类来进行存储：**

* 基础类型存储在栈内存，被引用或拷贝时，会创建一个完全相等的变量；
* 引用类型存储在堆内存，存储的是地址，多个引用指向同一个地址，这里会涉及一个“共享”的概念。

**js检测数据类型的方法有很多种，包括 typeof、instanceof、 Object.prototype.toString、constructor下面分别介绍下这几种方法**

## 第一种判断方法：typeof

**这是比较常用的一种，那么我们通过一段代码来快速回顾一下这个方法。**

``` javascript
  typeof 1    // 'number'
  typeof '1'  // 'string'
  typeof undefined    // 'undefined'
  typeof true         // 'boolean'
  typeof Symbol()     // 'symbol'
  typeof null     // 'object'
  typeof []   // 'object'
  typeof {}   // 'object'
  typeof console  // 'object'
  typeof console.log  // 'function'
```

**注意**

* 该方法检测null会返回object，不代表 null 就是引用数据类型，并且 null 本身也不是对象，如果使用在判断语句中可以直接用"===null"来判断
* 引用数据类型 Object，用 typeof 来判断的话，除了 function 会判断为 OK 以外，其余都是 'object'，是无法判断出来的

## 第二种判断方法：instanceof

**instanceof 方法，我们 new 一个对象，那么这个新对象就是它原型链继承上面的对象了，通过 instanceof 我们能判断这个对象是否是之前那个构造函数生成的对象，这样就基本可以判断出这个新对象的数据类型**

``` javascript
  let Fn = function() {}
  let gs = new Fn()
  gs instanceof Fn // true
  let gcs = new String('Mercedes Gs')
  gcs instanceof String // true
  let str = 'string'
  str instanceof String // false
```

**其实自己也可以手写一个instanceof方法**

``` javascript
function newInstanceof(val, type) {
    // 这里先用typeof来判断基础数据类型，如果是，直接返回false
    if (typeof val !== 'object' || val === null) return false;
    // getProtypeOf是Object对象自带的API，能够拿到参数的原型对象
    let proto = Object.getPrototypeOf(val);
    while (true) {                  //循环往下寻找，直到找到相同的原型对象
        if (proto === null) return false;
        if (proto === type.prototype) return true;//找到相同原型对象，返回true
        proto = Object.getPrototypeof(proto);
    }
}
// 验证newInstanceof方法是否有效
console.log(newInstanceof(new Number(123), Number));    // true
console.log(newInstanceof(123, Number));                // false
```

**注意**

* instanceof 可以准确地判断复杂引用数据类型，但不能正确判断基础数据类型

**以上两种方法单独使用都无法满足所有场景的判断，只有两者混写。下面看下第三种方法**

## 第三种判断方法：Object.prototype.toString

**toString() 是 Object 的原型方法，调用该方法，可以统一返回格式为 “[object Xxx]” 的字符串，其中 Xxx 就是对象的类型。对于 Object 对象，直接调用 toString() 就能返回 [object Object]；而对于其他对象，则需要通过 call 来调用，才能返回正确的类型信息。还是先看代码吧**

``` javascript
  Object.prototype.toString({})       // "[object Object]"
  Object.prototype.toString.call({})  // 同上结果，加上call也ok
  Object.prototype.toString.call(1)    // "[object Number]"
  Object.prototype.toString.call('1')  // "[object String]"
  Object.prototype.toString.call(true)  // "[object Boolean]"
  Object.prototype.toString.call(function(){})  // "[object Function]"
  Object.prototype.toString.call(null)   //"[object Null]"
  Object.prototype.toString.call(undefined) //"[object Undefined]"
  Object.prototype.toString.call(/123/g)    //"[object RegExp]"
  Object.prototype.toString.call(new Date()) //"[object Date]"
  Object.prototype.toString.call([])       //"[object Array]"
  Object.prototype.toString.call(document)  //"[object HTMLDocument]"
  Object.prototype.toString.call(window)   //"[object Window]"
```

**注意**

- 该方法可以很好的判断引用数据类型，包括浏览器窗口window和document
- 该方法最后返回统一字符串格式为 "[object Xxx]" ，而这里字符串里面的 "Xxx" ，第一个首字母要大写（注意：使用 typeof 返回的是小写）

**我们可以封装一个工具函数用来检测数据类型**

``` javascript
function ifType(val){
  let type  = typeof val;
  if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
    return type;
  }
  // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
  return Object.prototype.toString.call(val).replace(/^\[object (\S+)\]$/, '$1');
}
```

## 第四种判断方法：constructor

**constructor在其对应对象的原型下面，是自动生成的。当我们写一个构造函数的时候，程序会自动添加：构造函数名.prototype.constructor = 构造函数名**

``` javascript
  var str = 'hello';
  alert(str.constructor == String);//true
  var bool = true;
  alert(bool.constructor == Boolean);//true
  var num = 123;
  alert(num.constructor == Number);//true
  var nul = null;
  alert(nul.constructor == Object);//报错
  var und = undefined;
  alert(und.constructor == Object);//报错
  var oDate = new Date();
  alert(oDate.constructor == Date);//true
  var json = {};
  alert(json.constructor == Object);//true
  var arr = [];
  alert(arr.constructor == Array);//true
  var reg = /a/;
  alert(reg.constructor == RegExp);//true
  var fun = function () { };
  alert(fun.constructor == Function);//true
  var error = new Error();
  alert(error.constructor == Error);//true
```

**注意**

* null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断
* 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object
* 在重写对象原型时一般都需要重新给constructor赋值，以保证对象实例的类型不被篡改

### 总结

* typeof 可以判断基本数据类型null除外,引用数据类型除了function，其余也不能准确判断

* instanceof 可以准确地判断复杂引用数据类型，但不能正确判断基础数据类型

* Object.prototype.toString 很好的判断引用数据类型，包括浏览器窗口window和document

* constructor 是不稳定的，开发者一旦重写prototype原有的constructor引用会丢失，需要重新给constructor赋值。个人不推荐使用
