---
title: ES6变量的声明
date: 2019-10-10 19:38:42
---
## 1. 三种变量声明的方法以及对应特点

### var
1. 可以重复声明
2. 变量的声明会被提升
3. 没有局部作用域

### let【变量的声明】
1. 不可以重复声明
2. 变量的声明不会被提升
3. 具有局部作用域

### const【常量声明】
1. 不可以重复声明
2. 变量的声明不会被提升
3. 具有局部作用域
4. 常量的值无法改变

## 2. 解构（模式匹配）

**可以一次性从对象或者数组中获取多个值，并且把这些值赋值给不同的变量**

### 对象结构

``` javascript
let {name,age} = {name:"terry", age:12};           //name:terry,age:12
let {name:name,age:age} = {name:"terry",age:12}    //name:terry,age:12
let {name:name,age:gender} = {name:"terry",age:12} //name:terry,gender:12
let {name,age:gender} = {name:"terry",age:12}      //name:terry,gender:12

----------------------------------------------------------------------

let obj = {
    realname:"terry",
    address:{
        province:"山西省",
        city:"太原市",
        area:"尖草坪"
    }
}

let {realname,address:{city}} = obj;        //realname:terry, city:"太原市"
```

### 数组解构

``` javascript
let [a,b,c] = [8,2,5];      //a:8, b:2, c:5
let [a,a1,a2,[b,b2,b3]] = [8,2,5,[1,2,3],4];        //a:8, a1:2, a2:5, [b:1, b2:2, b3:3]
```
