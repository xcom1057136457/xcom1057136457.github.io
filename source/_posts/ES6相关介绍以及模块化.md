---
title: ES6相关介绍以及模块化
date: 2019-10-10 18:52:39
---
## 1. ES6介绍

**ES6是ES5的升级版，提供了简洁的语法和新的特性。ES6在浏览器上兼容性差一些，但是在nodejs上可以完全兼容**

**ES6 ------babel/webpack(js插件，运行在nodejs上)-------> ES5**

## 2. 模块化

### 1） 数据驱动（vue）

**ajax ------>根据数据创建虚拟的don节点，追加到页面中**

### 2） dom驱动

这里以表格为例
**ajax -> tr -> clone -> 数据填充**

### 3） 模块定义

**js文件、目录、目录嵌套都属于一个模块**

```
1. module变量
    在任意一个js文件中，都包含一个变量叫做module，module表示当前模块的一些信息

            id
            filename：完整路径 + 文件名
            paths：require的时候去哪里寻找要加载的模块
            parent：父模块
            children：子模块
            exports：对外暴露的对象，require当前模块实际上就是require这个exports对象
```

```
2. require()
    加载其他模块
    
    参数：
        1. 参数为路劲
            require('./module')
            按照指定路径加载需要的模块
        2. 参数为模块名称
            require('module')
            按照module.paths中指定的路径寻找该模块
```

```
3. node_modules
    目录，用于存放第三方模块
```

```
4. 模块可以为一个目录
    $ mkdir module
    $ cd module
    $ npm init
        将该目录初始化一个为node模块
        基本信息都存放在package.json
```

```
5. npm
    node的模块管理机制，node package manager
    
    npm init
        将当前目录初始化为一个node模块
        如果不想填写信息则可以使用 $npm init -y 直接跳过填写信息
    
    npm install xxx
        安装第三方模块xxx，局部安装，将第三方依赖默认安装到当前目录的node_modules中

        $ npm install xxx --save(默认)
        $ npm install xxx -S(默认)
            --save表示将模块安装到当前目录的node_modules;将这个依赖的信息添加到package.json中dependencies属性中，dependencies中的依赖为产品依赖
        $ npm install xxx --save-dev
            devDependencies开发依赖，只在产品开发阶段才会使用到，在产品阶段无需这些依赖

    npm install xxx -g
        -g：全局安装，将第三方依赖安装
            $ NODE_HOME/lib/node_modules
            $ NODE_HOME/bin
            $ NODE_HOME/share
```

```
6. cnpm
    因为npm是国外在管理，所以国内下载一些模块速度会很慢，所以淘宝做了一个npm的镜像仓库，为cnpm

    安装cnpm
        $ npm install -g cnpm --registry=https://registry.npm.taobao.org

    报错
        EACCES: permission denied, access '/opt/node-v10.14.2/lib/node_modules/cnpm/node_modules/address'
        报错原因是因为当前用户是普通用户，普通用户不允许操作非家目录
    
    解决方案
        1. sudo
        2. 将/opt/node-v10.14.2/lib/node_modules,/opt/node-v10.14.2/bin,/opt/node-v10.14.2/share 这三个目录的拥有者设置为当前用户
        $ sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

### 4）babel
**将es6转换为es5，但是注意一点：如果你写的js代码运行在nodejs【服务器，插件】上无需转换**

``` shell
1. 安装babel
    $ cnpm install -g babel-cli
    $ babel --version

2. 本地安装预设
    $ cnpm install --save-dev babel-preset-es2015
```
