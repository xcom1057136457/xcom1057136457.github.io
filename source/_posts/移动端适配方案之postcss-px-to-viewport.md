---
title: 移动端适配方案之postcss-px-to-viewport
date: 2020-08-15 23:38:30
---
> 之前做移动端适配时，基本上是采用rem方案，现在发现了一个新的方案，就是用viewport单位，现在`viewport`单位越来越受到众多浏览器的支持

postcss-px-to-viewport，将px单位自动转换成viewport单位，用起来超级简单，[postcss-px-to-viewport 文档](http://npm.taobao.org/package/postcss-px-to-viewport)

### 安装

`npm install postcss-px-to-viewport *--save-dev*` 

### 引入vue项目，再postcss.config.js引入

``` javascript

module.exports = {
  plugins: {
    autoprefixer: {},
    'postcss-px-to-viewport': {
      viewportWidth: 750,   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3,     // 指定`px`转换为视窗单位值的小数位数
      viewportUnit: "vw",   //指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore'],// 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1,     // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false     // 允许在媒体查询中转换`px`
    }
  }
}
```

### 参数解析

``` javascript

{
  unitToConvert: 'px'
  viewportWidth: 320,
  unitPrecision: 5,
  propList: ['*'],
  viewportUnit: 'vw',
  fontViewportUnit: 'vw',
  selectorBlackList: [],
  minPixelValue: 1,
  mediaQuery: false,
  replace: true,
  exclude: [],
  landscape: false,
  landscapeUnit: 'vw',
  landscapeWidth: 568
}
```

* `unitToConvert(String)` 要转换的单位，默认是'px'
* `viewportWidth(Number) `viewport的宽度，默认是320，可根据设计稿来，750的设计稿就写750
* `unitPrecision(Number)` 指定`px`转换为视窗单位值的小数位数，默认是5
* `propList(Array)` 指定可以转换的css属性，默认是['*']，代表全部属性进行转换
  1. 精确匹配
  2. \* 代表全部属性
  3. 在字符串前面或者后面用*，如 ['**position**'] 会匹配background-position-y
  4. 用！则该属性排除. 如: ['*', '!letter-spacing']
  5. Combine the "not" prefix with the other prefixes. Example: ['*', '!font*']
* `viewportUnit(String)`指定需要转换成的视窗单位，默认vw
* `fontViewportUnit(String)`指定字体需要转换成的视窗单位，默认vw
* `selectorBlackList(Array)` 指定不转换为视窗单位的类，保留px，值为string或正则regexp，建议定义一至两个通用的类名
  1. 值为string类型， 检查字符是否包含['body'] 匹配 .body-class
  2. 值为regexp类型，正则匹配[/^body$/]匹配body而不是 .body
* `minPixelValue(Number)`默认值1，小于或等于`1px`不转换为视窗单位
* `mediaQuery(Boolean)`是否在媒体查询时也转换px，默认false
* `replace(Boolean)` replaces rules containing vw instead of adding fallbacks.
* `exclude(Array or Regexp)` 设置忽略文件，如node_modules
  1. 如果是regexp, 忽略全部匹配文件.
  2. 如果是数组array, 忽略指定文件.

### 可能遇到的问题

@keyframes 和media查询里的px默认是不转化的，设置mediaQuery： true则媒体查询里也会转换px

@keyframes可以暂时手动填写vw单位的转化结果
