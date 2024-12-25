---
title: localStorage容量太小?试试它们
date: 2023-01-31 16:50:47
---
`localStorage` 是前端本地存储的一种，其容量一般在 `5M-10M` 左右，用来缓存一些简单的数据基本够用，毕竟定位也不是大数据量的存储。

在某些场景下 `localStorage` 的容量就会有点捉襟见肘，其实浏览器是有提供大数据量的本地存储的如 `IndexedDB` 存储数据大小一般在 `250M` 以上。

弥补了`localStorage`容量的缺陷，但是使用要比`localStorage`复杂一些 [mdn IndexedDB](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FIndexedDB_API)

不过已经有大佬造了轮子封装了一些调用过程使其使用相对简单，下面我们一起来看一下

## localforage

[localforage](https://link.juejin.cn/?target=https%3A%2F%2Flocalforage.docschina.org%2F%23api-config) 拥有类似 `localStorage` API，它能存储多种类型的数据如 **`Array` `ArrayBuffer` `Blob` `Number` `Object` `String`**，而不仅仅是字符串。

这意味着我们可以直接存 对象、数组类型的数据避免了 `JSON.stringify` 转换数据的一些问题。

存储其他数据类型时需要转换成上边对应的类型，比如vue3中使用 `reactive` 定义的数据需要使用`toRaw` 转换成原始数据进行保存， `ref` 则直接保存 `xxx.value` 数据即可。

## 安装

[下载最新版本](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmozilla%2FlocalForage%2Freleases) 或使用 `npm` `bower` 进行安装使用。

```shell
# 引入下载的 localforage 即可使用
<script src="localforage.js"></script>
<script>console.log('localforage is: ', localforage);</script>

# 通过 npm 安装：
npm install localforage

# 或通过 bower：
bower install localforage
```

## 使用

提供了与 `localStorage` 相同的api，不同的是它是异步的调用返回一个 `Promise` 对象

```javascript
localforage.getItem('somekey').then(function(value) {
    // 当离线仓库中的值被载入时，此处代码运行
    console.log(value);
}).catch(function(err) {
    // 当出错时，此处代码运行
    console.log(err);
});

// 回调版本：
localforage.getItem('somekey', function(err, value) {
    // 当离线仓库中的值被载入时，此处代码运行
    console.log(value);
});
```

## 提供的方法有

- `getItem` 根据数据的 `key` 获取数据 差不多返回 `null`
- `setItem` 根据数据的 `key` 设置数据（存储`undefined`时getItem获取会返回 `null`）
- `removeItem` 根据key删除数据
- `length` 获取key的数量
- `key` 根据 key 的索引获取其名
- `keys` 获取数据仓库中所有的 key。
- `iterate` 迭代数据仓库中的所有 `value/key` 键值对。

## 配置

[完整配置可查看文档](https://link.juejin.cn/?target=https%3A%2F%2Flocalforage.docschina.org%2F%23api-config) 这里说个作者觉得有用的

`localforage.config({ name: 'My-localStorage' });` 设置仓库的名字，不同的名字代表不同的仓库，当一个应用需要多个本地仓库隔离数据的时候就很有用。

```javascript
const store = localforage.createInstance({
    name: "nameHere"
});

const otherStore = localforage.createInstance({
    name: "otherName"
});

// 设置某个数据仓库 key 的值不会影响到另一个数据仓库
store.setItem("key", "value");
otherStore.setItem("key", "value2");
```

同时也支持删除仓库

```javascript
// 调用时，若不传参，将删除当前实例的 “数据仓库” 。
localforage.dropInstance().then(function() {
  console.log('Dropped the store of the current instance').
});

// 调用时，若参数为一个指定了 name 和 storeName 属性的对象，会删除指定的 “数据仓库”。
localforage.dropInstance({
  name: "otherName",
  storeName: "otherStore"
}).then(function() {
  console.log('Dropped otherStore').
});

// 调用时，若参数为一个仅指定了 name 属性的对象，将删除指定的 “数据库”（及其所有数据仓库）。
localforage.dropInstance({
  name: "otherName"
}).then(function() {
  console.log('Dropped otherName database').
});
```

# idb-keyval

`idb-keyval`是用`IndexedDB`实现的一个超级简单的基于 `promise` 的键值存储。

## 安装

npm `npm install idb-keyval`

```javascript
// 全部引入
import idbKeyval from 'idb-keyval';

idbKeyval.set('hello', 'world')
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));
  
// 按需引入会摇树
import { get, set } from 'idb-keyval';

set('hello', 'world')
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));
  
get('hello').then((val) => console.log(val));

```

浏览器直接引入 `<script src="https://cdn.jsdelivr.net/npm/idb-keyval@6/dist/umd.js"></script>`

暴露的全局变量是 `idbKeyval` 直接使用即可。

## 提供的方法

由于其没有中文的官网，会把例子及自己的理解附上

### set 设置数据

值可以是 `数字、数组、对象、日期、Blobs等`，尽管老Edge不支持null。

键可以是`数字、字符串、日期`，（IDB也允许这些值的数组，但IE不支持）。

```js
import { set } from 'idb-keyval';

set('hello', 'world')
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));

```

### setMany 设置多个数据

一个设置多个值，比一个一个的设置更快

```js
import { set, setMany } from 'idb-keyval';

// 不应该:
Promise.all([set(123, 456), set('hello', 'world')])
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));

// 这样做更快:
setMany([
  [123, 456],
  ['hello', 'world'],
])
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));

```

### get 获取数据

如果没有键，那么`val`将返回`undefined`的。

```js
import { get } from 'idb-keyval';

// logs: "world"
get('hello').then((val) => console.log(val));

```

### getMany 获取多个数据

一次获取多个数据，比一个一个获取数据更快

```js
import { get, getMany } from 'idb-keyval';

// 不应该:
Promise.all([get(123), get('hello')]).then(([firstVal, secondVal]) =>
  console.log(firstVal, secondVal),
);

// 这样做更快:
getMany([123, 'hello']).then(([firstVal, secondVal]) =>
  console.log(firstVal, secondVal),
);

```

### del 删除数据

根据 `key` 删除数据

```js
import { del } from 'idb-keyval';

del('hello');

```

### delMany 删除多个数据

一次删除多个键，比一个一个删除要快

```js
import { del, delMany } from 'idb-keyval';

// 不应该:
Promise.all([del(123), del('hello')])
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));

// 这样做更快:
delMany([123, 'hello'])
  .then(() => console.log('It worked!'))
  .catch((err) => console.log('It failed!', err));

```

### update 排队更新数据，防止由于异步导致数据更新问题

因为 `get` 与 `set` 都是异步的使用他们来更新数据可能会存在问题如：

```js
// Don't do this:
import { get, set } from 'idb-keyval';

get('counter').then((val) =>
  set('counter', (val || 0) + 1);
);

get('counter').then((val) =>
  set('counter', (val || 0) + 1);
);

```

上述代码我们期望的是 `2` 但实际结果是 `1`，我们可以在第一个回调执行第二次操作。

更好的方法是使用 `update` 来更新数据

```js
// Instead:
import { update } from 'idb-keyval';

update('counter', (val) => (val || 0) + 1);
update('counter', (val) => (val || 0) + 1);

```

将自动排队更新，所以第一次更新将计数器设置为`1`，第二次更新将其设置为`2`。

### clear 清除所有数据

```js
import { clear } from 'idb-keyval';

clear();

```

### entries 返回 `[key, value]` 形式的数据

```js
import { entries } from 'idb-keyval';

// logs: [[123, 456], ['hello', 'world']]
entries().then((entries) => console.log(entries));

```

### keys 获取所有数据的 `key`

```js
import { keys } from 'idb-keyval';

// logs: [123, 'hello']
keys().then((keys) => console.log(keys));

```

### values 获取所有数据 `value`

```js
import { values } from 'idb-keyval';

// logs: [456, 'world']
values().then((values) => console.log(values));

```

### createStore 自定义仓库

文字解释：表 === store === 商店 一个意思

```js
// 自定义数据库名称及表名称
// 创建一个数据库: 数据库名称为 tang_shi， 表名为 table1
const tang_shi_table1 = idbKeyval.createStore('tang_shi', 'table1')

// 向对应仓库添加数据
idbKeyval.set('add', 'table1 的数据', tang_shi_table1)

// 默认创建的仓库名称为 keyval-store 表名为 keyval
idbKeyval.set('add', '默认的数据')

```

![截屏2022-11-06 下午8.40.58.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02592f01a8cc45ea92ab65161da5f23d~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

使用 `createStore` 创建的数据库一个库只会创建一个表即：

```js
// 同一个库有不可以有两个表，custom-store-2 不会创建成功:
const customStore = createStore('custom-db-name', 'custom-store-name');
const customStore2 = createStore('custom-db-name', 'custom-store-2');

// 不同的库 有相同的表名 这是可以的:
const customStore3 = createStore('db3', 'keyval');
const customStore4 = createStore('db4', 'keyval');

```

### promisifyRequest

自己管理定制商店，这个没搞太明白，看[文档](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjakearchibald%2Fidb-keyval%2Fblob%2Fmain%2Fcustom-stores.md)中说既然都用到这个了不如直接使用[idb 这个库](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fidb)

## 总结

本文介绍了两个 `IndexedDB` 的库，用来解决 `localStorage` 存储容量太小的问题

`localforage` 与 `idb-keyval` 之间我更喜欢 `localforage` 因为其与 `localStorage` 相似的api几乎没有上手成本。

如果需要更加灵活的库可以看一下 [dexie.js](https://link.juejin.cn/?target=https%3A%2F%2Fwww.dexie.org%2F)、[PouchDB](https://link.juejin.cn/?target=https%3A%2F%2Fpouchdb.com%2F)、[idb](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fidb)、[JsStore](https://link.juejin.cn/?target=https%3A%2F%2Fjsstore.net%2F) 或者 [lovefield](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fgoogle%2Flovefield) 之类的库
