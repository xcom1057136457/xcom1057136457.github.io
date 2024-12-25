---
title: ES6数组(Array)新特性
date: 2019-10-11 15:13:55
---
## 1. 数组的创建方式

### 直接创建
`let arr = new Array(3,2)`

### 使用rest运算符
``` javascript
let str = "Hello World";
[...str];
```

### 使用Array.from(v)【v为类数组对象或者可遍历的对象】
``` javascript
let array_like = {"0" : "terry", "1" : "larry", length : 2}
console.log(array_like);

// 从数组对象中解构出来slice方法
let {slice} = [];

// 1. 使用原始的Array.prototype.slice转换
console.log(slice.call(array_like,0));
等价于
console.log(Array.prototype.slice.call(array_like,0));

// 2. 使用Array.from转换
console.log(arr_like);
console.log(Array.from(array_like));

// 3. 使用Array.from转换可遍历的对象
let set = new Set([1,2,3,1,2,4,5,6]);
console.log(set);
console.log(Array.from(set));
```

### 使用Array.of(p1,p2,....)将参数中的元素转换为数组

## 2. 方法

### Array.prototype.includes
**检测数组中是否包含某个元素，返回的是Boolean值**

### Array.prototype.find(function(){})
**检测数组中是否包含某个值或符合某个条件，起筛选作用**   

举例:
``` javascript
let arr = [1,2,3,4];
arr.find(item => item > 2)  //返回第一个大于2的值
```

### Array.prototype.findIndex
**检测数组中是否包含某个值或符合某个条件，起筛选作用**

举例:
``` javascript
let arr = [1,2,3,4];
arr.findIndex(item => item > 2)  //返回第一个大于2的值的索引
```

### Array.prototype.fill
**用一个元素填充数组**

### Array.prototype.keys
**获取一个数组的所有索引**

### Array.prototype.values
**获取一个数组的所有值**

### Array.prototype.entries
**获取一个数组的所有索引和值**

## 3. 数组的迭代
``` javascript
let arr = ["terry","larry","tom","jacky"];

// 1. 获取迭代器
let values_iterator = arr.values();

// 2. 通过迭代器获取数组中的元素
let item;
while(!(item = values_iterator.next()).done){
    console.log(item.value);
}

// 3. 使用for-of遍历迭代器
let entry_iterator = arr.entries();
for(let entry of entry_iterator){
    console.log(entry);
}

// 4. 使用for-of遍历数组
for(let item of arr){
    console.log(item);
}
```

## 4. 集合api

### Set

**无序不可以重复的集合【天然去重】(数组中的元素可以重复)**

``` javascript
1. 实例化Set对象   
    let set = new Set();
    let set = new Set([1,2,3,1,2])

2. Set.prototype.xxx
    size            //set集合中元素的个数
    delete(val)     //从集合中删除val
    has(val)        //判断集合中是否存在val
    add(val)        //向集合中添加val
    clear()         //清空
    forEach()       //遍历
    keys()          //迭代器
    values()        //迭代器
    entries()       //迭代器
```

### Map

**key可以为任意数据对象（对象的key只能为字符串）**

``` javascript
1. 实例化map对象
    let map = new Map()
    let map = new Map(entry)       //entry格式[["",""],["",""],.....]

2. Map.prototype.xxx
    size            // Map集合中元素的个数
    set(key,val)    // 向Map中添加键值对，key不可以重复，如果重复，value更新
    get(key)        // 通过key获取value
    has(key)        // 判断map集合中是否存在指定的key
    delete(key)     // 通过key从Map集合中删除
    clear()         // 清空Map集合
    keys()          //迭代器
    values()        //迭代器
    entries()       //迭代器
```
