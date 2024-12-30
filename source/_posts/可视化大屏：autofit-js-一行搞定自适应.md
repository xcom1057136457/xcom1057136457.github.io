---
title: 可视化大屏：autofit.js 一行搞定自适应
date: 2024-12-30 10:12:36
---
## 可视化大屏适配/自适应现状

可视化大屏的适配是一个老生常谈的话题了，现在其实不乏一些大佬开源的自适应插件、工具但是我为什么还要重复造轮子呢？因为目前市面上适配工具每一个都无法做到完美的效果，做出来的东西都差不多，最终实现效果都逃不出白边的手掌心，可以解决白边问题的，要么太过于复杂，要么会影响dom结构。

### 三大常用方式

1. vw/vh方案

   1. 概述：按照设计稿的尺寸，将px按比例计算转为vw和vh
   
   2. 优点：可以动态计算图表的宽高，字体等，灵活性较高，当屏幕比例跟 ui 稿不一致时，不会出现两边留白情况

   3. 缺点：每个图表都需要单独做字体、间距、位移的适配，比较麻烦

2. scale方案

   1. 概述：也是目前效果最好的一个方案

   2. 优点：代码量少，适配简单 、一次处理后不需要在各个图表中再去单独适配.
   
   3. 缺点：留白，有事件热区偏移，下面介绍的autofit.js已经完全解决了此问题

3. rem + vw vh方案

   1. 概述：这名字一听就麻烦，具体方法为获得 rem 的基准值 ，动态的计算html根元素的font-size ，图表中通过 vw vh 动态计算字体、间距、位移等
   
   2. 优点：布局的自适应代码量少，适配简单
   
   3. 缺点：留白，有时图表需要单独适配字体

基于此背景，我决定要造一个简单又好用的轮子。

## 解决留白问题

留白问题是在使用scale时才会出现，而其他方式实现起来又复杂，效果也不算太理想，总会破坏掉原有的结构，可能使元素挤在一起，所以我们还是选择使用scale方案，不过这次要做出一点小小的改变。

### 常用分辨率

首先来看一下我的拯救者的分辨率： 它可以代表从1920往下的分辨率

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241230101611.png)

我们可以发现，比例分别是：1.77、1.6、1.77、1.6、1.33... 总之，没有特别夸张的宽高比。

### 计算补齐白边所需的px

只要没有特别夸张的宽高比，就不会出现特别宽或者特别高的白边，那么我们能不能直接将元素宽高补过去？也就是说，当屏幕右侧有白边时，我们就让宽度多出一个白边的px，当屏幕下方有白边时，我们就让高度多出一个白边的px。

很喜欢CSGO玩家的一句话："啊？"

先想一下，如果此时按宽度比例缩放，会在下方留下白边，所以设置一下它的高度，设置多少呢？比如 scale==0.8 ，也就是说整个#app缩小了0.8倍，我们需要将高扩大多少倍才可以回到原来的大小呢？

emmm.....

算数我最不在行了，启动高材生

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241230101659.png)

原来是八分之十，我vue烧了。

当浏览器窗口比设计稿大或者小的时候，就应该触发缩放，但是比例不一定，如果按照scale等比缩放时，宽度从1920缩小0.8倍也就是1536，而高度缩小0.8也就是743，如果此时浏览器高度过高，那么就会出现下方的白边，根据高材生所说的，缩小0.8后只需要放大八分之十就可以变回原大小，所以以现在的高度743*1.25=928，使宽度=928px就可以完全充满白边！

思路是正确的，但是能不能再简单一点

是浏览器高度！我忽略了浏览器高度，我可以直接使用浏览器高度乘以1.25然后再缩放达0.8！就是 1 ！

也就是说 clientHeight / scale 就等于我们需要的高度！

我们用代码试一试（autofit.js初代核心代码）

```` javascript
function keepFit(designWidth, designHeight, renderDom) {
  let clientHeight = document.documentElement.clientHeight;
  let clientWidth = document.documentElement.clientWidth;
  let scale = 1;
  if (clientWidth / clientHeight < designWidth / designHeight) {
    scale = (clientWidth / designWidth)
    document.querySelector(renderDom).style.height = `${clientHeight / scale}px`;
  } else {
    scale = (clientHeight / designHeight)
    document.querySelector(renderDom).style.width = `${clientWidth / scale}px`;
  }
  document.querySelector(renderDom).style.transform = `scale(${scale})`;
}
````

解释一下：

参数分别是：设计稿的宽高和你要适配的元素，在vue中可以直接传#app。

下面的if判断的是宽度固定还是高度固定，当屏幕宽高比小于设计宽高比时，

我们把高度写成 clientHeight / scale ，宽度也是同理。

### 最终效果

将这段代码放到App.vue的mounted运行一下

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241230101856.png)

如上图所示：我们成功了，我们仅用了1 2 3 4....这么几行代码，就做到了足以媲美复杂写法的自适应！

我把这些东西封装了一个npm包：[autofit.js](https://www.npmjs.com/package/autofit.js) ，开箱即用，欢迎下载！

## 亲手打造集成工具：autofit.js

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241230101946.png)

这是一款可以使你的项目一键自适应的工具 [github源码👉go](https://github.com/Auto-Plugin/autofit.js)

* 从npm下载
```` shell
npm i autofit.js
````

* 引入
```` javascript
import autofit from 'autofit.js'
````

* 快速开始
```` javascript
autofit.init()
````

> 默认参数为1920*929（即去掉浏览器头的1080）, 直接在大屏启动时调用即可

* 使用
```` javascript
export default {  
  mounted() {
	autofit.init({
        dh: 1080,
        dw: 1920,
        el:"body",
        resize: true
    })
  },
}
````

   * el：渲染的dom，默认是 "body"

   * dw：设计稿的宽度，默认是 1920

   * dh：设计稿的高度，默认是 1080

   * resize：是否监听resize事件，默认是 true

   * ignore：忽略缩放的元素（该元素将反向缩放），参数见readme.md

   * transition：过渡时间，默认是 0

   * delay：默认是 0

   * limit：默认是 0.1,当缩放阈值不大于此值时不缩放，比如设置为0.1时，0.9-1.1的范围会被重置为1

注意参数可能有变化，使用时请阅读最新版 readme.md 文档

最新版 v3.2.x 支持更多功能，请访问：[https://auto-plugin.github.io/index/autofit.js/](https://auto-plugin.github.io/index/autofit.js/)

