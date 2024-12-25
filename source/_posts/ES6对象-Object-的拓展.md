---
title: ES6对象(Object)的拓展
date: 2019-10-11 13:57:17
---
## 1. 对象的简写

``` javascript
let name = "terry",
let age = 12,
let sayName = function(){
    console.log(this.name);
}

ES5写法:
    let obj = {
        name : "terry",
        age : 12,
        sayName : function(){
            console.log(this.name);
        }
    }

ES6写法:
    let obj = {
        name,
        age,
        sayName
    }
```

## 2. 方法

**实例可以调用该实例构造函数原型中的方法，但不能调用构造函数中的方法**

### Object.is({},{})
**类似于 ===**

### Object.assign(target,o1,o2)
**将o1,o2中可枚举的属性合并到target中，返回target**

### Object.getPrototypeOf(obj)
**获取obj对象的原型也就是其构造函数的原型**

### Object.setPrototypeOf(obj,prototype)
**为obj对象指定一个新的原型等价于**`obj.__proto__ = prototype`

### Object.keys({})
**获取一个对象的所有属性名**

### Object.values({})
**获取一个对象的所有属性值**

### Object.entries({})
**获取一个对象的所有属性名与属性值，并以[[],[]]方式显示**
