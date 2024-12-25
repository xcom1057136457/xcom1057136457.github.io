---
title: ES6承诺函数(Promise)
date: 2019-10-11 18:35:55
---
## 1. 实例化

> 承诺对象，用于封装异步操作

```
状态：
    pengding: 正在执行内部代码
    resolved: 运行成功
    rejected: 运行失败

实例：
    let promise = new Promise((resolve, reject) => {

        //异步操作
        当异步操作成功的时候执行resolve(),就可以将承诺的状态由 pending -> resolved

        当异步操作失败的时候执行reject(),就可以将承诺的状态由pending -> rejected

    })

    当promise实例产生，这个Promise中的回调函数就会执行。通过then方法监听承诺对象状态的改变
```

## 2. 方法

### Promise原型中的方法
```
1. then(resolved_callback, rejected_callback)
    resolved_callback：当承诺对象状态pending -> resolved
    rejected_callback：当承诺对象状态pending -> rejected

2. catch(rejected_callback)
    rejected_callback：当承诺对象的状态pending -> rejected
    不管是Ajax交互中出现的404，500异常会被catch捕获，在then中的代码语法错误也会被catch捕获

```

### Promise构造函数中的方法
```
1. Promise.all()
    let promise = Promise.all(p1,p2,p3,.....)   //需要p1、p2、p3全部完成以后状态才会变成完成状态

2. Promise.race()
    let promise = Promise.race(p1,p2,p3,.....)  //p1、p2、p3不需要全部成功，选取运行最快的状态

3. Promise.resolve()
    将参数对象转换为一个承诺对象

4. Promise.reject()
    返回一个状态为rejected的承诺对象

```

