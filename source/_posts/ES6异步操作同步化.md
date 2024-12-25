---
title: ES6异步操作同步化
date: 2019-10-12 14:16:06
---
## Generator函数

### 1. 声明
``` javascript
function* method_name(){
    yield xxx,
    yield xxx
}
```

### 2. yield表达式

* yield只能出现在generator函数中
* 默认情况下yield表达式的返回值为undefined

### 3. 调用

``` javascript
//Generator调用的结果为迭代器
let iterator = method_name();

//每次调用next方法可以依次获取每个yield的值
iterator.next()
```

### 4. Generator函数的应用

``` javascript
1. 可以生成迭代器对象
    let obj = {"0" : "terry", "1" : "larry"}

    问题：如何将obj转换为一个可迭代的对象
    思路：
        obj[Symbol.iterator] = [][Symbol.iterator]
        obj[Symbol.iterator] = 迭代器生成函数
    例如：
        let obj = {"0" : "terry", "1" : "larry", "2" : "tom"};
        obj[Symbol.iterator] = function* (){
            for(let key in obj){
                let val = obj[key];
                yield [key,val];
            }
        }

        obj.entries = obj[Symbol.iterator];
```

``` javascript
2. 利用Generator函数实现异步函数的同步化
    function* foo(){
        let customers = yield call($.get, c_url);
        let order = yield call($.get, o_url)
    }

    /*
        异步函数的执行器{
            1) 在上一个请求结束后再去调用下一个请求;
            2) 将当前请求结果左右yield表达式的返回值返回
        }
    */

    function call(handler, url){
        handler(url)
        .then((response) => {
            iterator.next(response);
        })
    }

    let iterator = foo();
    iterator.next();
```

## Async函数

> Generator函数的语法糖（对于Generator函数简化和功能增强）

``` javascript
async function foo(){
    let customers = await $.get(c_url);
    let address = await $.get(a_url);
    return [要返回的值];
}

let promise = foo();
promise.then((result) => {
    result为foo函数的返回值
})
```
