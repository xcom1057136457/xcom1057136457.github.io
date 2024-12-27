---
title: js原型，原型链知多少
date: 2021-07-07 15:26:03
---
## 构造函数创建对象

``` javascript
function Person() {

}
let person = new Person();
person.name = 'jimmy';
console.log(person.name) // jimmy
```

在这个例子中，Person 就是一个构造函数，我们使用 new 创建了一个实例对象。person是实例对象

## prototype

每个 **函数** 都有一个 prototype 属性。

``` javascript
function Person() {

}
// **prototype 是函数才会有的属性**
Person.prototype.name = 'Kevin';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name) // Kevin
console.log(person2.name) // Kevin
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/8470f0c0c8a742b4b6fab906d1ce1fc9%7Etplv-k3u1fbpfcp-watermark.image)

**原型定义：**
每一个JavaScript对象(null除外)在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型"继承"属性。

## **proto**

每一个JavaScript对象(除了 null )都具有的一个属性，叫__proto__，这个属性会指向该对象的原型

``` javascript
function Person() {

}
var person = new Person();
console.log(person.__proto__ === Person.prototype); // true
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/80dcd1fe38d24c14aa692c613a11a20c%7Etplv-k3u1fbpfcp-watermark.image)

## constructor

每个原型都有一个constructor 属性指向关联的构造函数。

``` javascript
function Person() {

}
console.log(Person === Person.prototype.constructor); // true
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/32eb59acc522484c9affc4194a01e440%7Etplv-k3u1fbpfcp-watermark.image)

以上就是 构造函数，原型和实例对象之间的关系。

**综上我们已经可以得出：**

``` javascript
function Person() {

}

var person = new Person();

console.log(person.__proto__ === Person.prototype) // true
console.log(Person.prototype.constructor === Person) // true
// 顺便学习一个ES5的方法,可以获得对象的原型
console.log(Object.getPrototypeOf(person) === Person.prototype) // true
```

## 实例对象与原型

当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。

``` javascript
function Person() {

}

Person.prototype.name = 'jimmy';
let person = new Person();
person.name = 'chimmy';
console.log(person.name) // chimmy
delete person.name;
console.log(person.name) // jimmy
```

在这个例子中，我们给实例对象 person 添加了 name 属性，当我们打印 person.name 的时候，结果自然为 chimmy。但是当我们删除了 person 的 name 属性时，读取 person.name，从 person 对象中找不到 name 属性就会从 person 的原型也就是 person.proto ，也就是 Person.prototype中查找，幸运的是我们找到了 name 属性，结果为 jimmy。
 但是万一还没有找到呢？原型的原型又是什么呢？

## 原型的原型

原型也是一个对象，既然是对象，我们就可以用最原始的方式创建它。

``` javascript
let obj = new Object();
obj.name = 'jimmy'
console.log(obj.name) // jimmy
```

其实原型对象就是通过 Object 构造函数生成的，结合之前所讲，实例的 **proto** 指向构造函数的 prototype ，所以我们更新下关系图

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/460f0b6e720c405a89f116a5bf4231b5%7Etplv-k3u1fbpfcp-watermark.image)

## 原型链

那 Object.prototype 的原型呢？是null，我们可以打印

``` javascript
console.log(Object.prototype.__proto__ === null) // true
```

所以查找属性的时候查到 Object.prototype 就可以停止查找了。null没有原型。
下图中由相互关联的原型组成的链状结构就是原型链，也就是蓝色的这条线。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/d19c4d6608b549e4b30b50e2563ae422%7Etplv-k3u1fbpfcp-watermark.image)

## 补充

**constructor**

``` javascript
function Person() {

}
var person = new Person();
console.log(person.constructor === Person); // true
```

当获取 person.constructor 时，其实 person 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 person 的原型也就是 Person.prototype 中读取，正好原型中有该属性，所以：person.constructor === Person.prototype.constructor

**真的是继承吗？**

每一个对象都会从原型继承属性，实际上，继承是一个十分具有迷惑性的说法，引用《你不知道的JavaScript》中的话，就是： 继承意味着复制操作，然而 JavaScript 默认并不会复制对象的属性，相反，JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。
