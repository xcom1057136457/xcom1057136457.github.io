---
title: ES6函数(Function)的拓展
date: 2019-10-11 14:24:36
---
## 函数简写

### 函数声明在对象中
``` javascript
ES5写法:
    let obj = {
        sayName:function(){
            console.log(this.name);
        }
    }

ES6写法:
    let obj = {
        sayName(){
            console.log(this.name);
        }
    }
```

### 函数声明在参数中（回调函数）【箭头函数】
**箭头函数中的this的取值为该箭头函数外部函数的this,如果箭头函数没有外部函数，那么这个this就指向全局对象**

**1.极简模式**
``` javascript
item表示形参，xxx为返回结果（方法体中只有这一个表达式）

item => xxx
等价于
function(item){return xxx}

例如从一个数组中筛选出满足条件的值
let result = arr.filter(item => item.gender === "male")
```

**2.普通模式**
``` javascript
当形参有多个的时候，参数必须添加小括号，当方法体中有多个表达式，方法体一定要加大括号

let result = arr.filter((item,index) => {
    console.log(index)
    return item.gender === "male"
})
```

**3.this指向**

``` javascript
let obj = {
    data:{
        name : "one",
        list : [1,3,4,2]
    },
    foo(){
        this.data.list.forEach((item) => {
            //箭头函数的this指向外部函数的this也就是obj
            console.log(this.data.name);
        })
    },
    bar(){
        let o = this;
        this.data.list.forEach(function(item){
            //回调函数的this指向global，而我们需要访问obj对象，所以提前先将obj保存到变量o中
            console.log(o.data.name);
        })
    }
}

obj.foo();
obj.bar();
```
