---
title: CSS-奇思妙想-全兼容的毛玻璃效果
date: 2024-08-29 16:49:21
---
## 什么是 `backdrop-filter`

`backdrop-filter` CSS 属性可以让你为一个元素后面区域添加图形效果（如模糊或颜色偏移）。 因为它适用于元素背后的所有元素，为了看到效果，必须使元素或其背景至少部分透明。

`backdrop-filter` 与 `filter` 非常类似，可以取的值都是一样的，但是一个是作用于整个元素，一个是只作用于元素后面的区域。

### `backdrop-filter` 与 `filter` 对比

我们使用 `backdrop-filter` 与 `filter` 同时实现一个毛玻璃效果作为对比，伪代码如下：

``` html
<div class="bg">
    <div>Normal</div>
    <div class="g-filter">filter</div>
    <div class="g-backdrop-filter">backdrop-filter</div>
</div>
```

``` css
.bg {
    background: url(image.png);
    
    & > div {
        width: 300px;
        height: 200px;
        background: rgba(255, 255, 255, .7);
    }
    .g-filter {
        filter: blur(6px);
    }
    .g-backdrop-filter {
        backdrop-filter: blur(6px);
    }
}
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241227145236.png)

[CodePen Demo -- filter 与 backdrop-filter 对比](https://codepen.io/Chokcoco/pen/WNjebrr)

在 `backdrop-filter` 之前，想实现上述的只给元素背景添加滤镜效果还是非常困难的，并且，对于静态画面还好，如果背景还是可以滚动的动态背景，通常 CSS 是无能为力的。

`backdrop-filter` 正是为了给元素后的内容添加滤镜而不影响元素本身而诞生的。使用它可以非常方便的实现磨砂玻璃效果（毛玻璃）！



## `backdrop-filter` 的兼容性

`backdrop-filter` 其实已经诞生挺久了，然而，firefox 至今都不兼容它！

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/3844c5f068fb498e84759db843394e64%7Etplv-k3u1fbpfcp-zoom-1.image)

对于部分已经放弃了 IE 的 PC 端业务而言，firefox 还是需要兼容的，想要让使用 `backdrop-filter` 实现毛玻璃效果应用落地，firefox 的兼容问题必须得解决。



## 在 firefox 中实现毛玻璃效果

OK，本文的重点就是在于如何在 firefox 中，不使用 `backdrop-filter` 而尽可能的还原毛玻璃的效果。

首先看一下，如果是正常使用 `backdrop-filter`，还是上述的例子效果如下，是没有毛玻璃效果的：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/6c3cf43d22c54ed6bd8f30c463723165%7Etplv-k3u1fbpfcp-zoom-1.image)

### 使用 background-attachment: fixed 兼容静态背景图

如果在 firefox 上想使用毛玻璃效果。应用毛玻璃元素的背景只是一张静态背景图，其实方法是有很多的。

我们只需在元素的背后，叠加一张同样的图片，利用 `background-attachment: fixed` 将叠加在元素下面的图片定位到与背景相同的坐标，再使用 `filter: blur()` 对其进行模糊处理即可。

伪代码如下：

``` html
<div class="g-glossy">frosted glass effect </div>
```

``` css
$img: 'https://static.pexels.com/photos/373934/pexels-photo-373934.jpeg';

body {
    height: 100vh;
    display: flex;
    background-image: url($img);
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: cover;
}

.g-glossy {
    position: relative;
    width: 600px;
    height: 300px;
    background-color: rgba(255, 255, 255, 0.5);
    overflow: hidden;
    z-index: 10;
    
    &::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-image: url($img);
        background-repeat: no-repeat;
        background-attachment: fixed;
        background-size: cover;
        filter: blur(10px);
        z-index: -1;
    }
}
```

效果如下：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/f515dbca75754973807a2f0e07b26e2e%7Etplv-k3u1fbpfcp-zoom-1.image)

此方法也是在没有 `backdrop-filter` 之前，在各个浏览器想实现简单毛玻璃效果最常用的方法之一。

[CodePen Demo -- 使用 background-attachment: fixed | filter: bulr() 实现毛玻璃效果](https://codepen.io/Chokcoco/pen/XWRrVma)

#### 使用 background-attachment: fixed 兼容静态背景图的缺点

不过这种方法也有两个缺点：

1. 由于使用了伪元素叠加了一层背景，因为层级关系，父元素的 background 是在最下层的，所以元素本身的背景色其实并没有被充分体现，可以对比下两种方法的实际效果图：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/bd5d549e72254958810f2559ef61e648%7Etplv-k3u1fbpfcp-zoom-1.image)

解决方案是再通过另外一个伪元素再叠加一层背景色，这个背景色应该是原本赋值给父元素本身的。

叠加之后的效果如下：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/ff360c5bd02b4a4fbd72977d73710a5d%7Etplv-k3u1fbpfcp-zoom-1.image)

[CodePen Demo -- 使用 background-attachment: fixed | filter: bulr() 实现毛玻璃效果优化](https://codepen.io/Chokcoco/pen/abWbzKG)

2. 上述效果已经非常接近了，硬要挑刺的话，就是应用了模糊滤镜的伪元素的边缘有白边瑕疵，这一点其实是滤镜本身的问题，也非常好解决，我们只需要将伪元素的范围扩大一点即可：

``` css
.g-glossy {
    overflow: hidden;
    ....
    &::before {
        content: "";
        position: absolute;
        top: -100px;
        left: -100px;
        right: -100px;
        bottom: -100px;
    }
}
```

定位的代码由 `top: 0px;` 改为 `top: -100px`，四个方位都是如此即可。如此一来，就能做到基本上是百分百的模拟。

### 使用 `moz-element()` 配合 `filter: blur()` 实现复杂背景毛玻璃效果

下面这种方法就非常巧妙了，正常而言，运用毛玻璃效果的背景元素，都不是一张图片那么简单！背后通常都是整个页面复杂的结构，多层 DOM 的嵌套。

那么通过叠加一张简单的图片，就无法奏效了，我们得想办法模拟整个 DOM 元素。

而恰好，在 Firefox 中，有这么一个属性 -- `-moz-element()`。

何为 `-moz-element()`？[MDN-element](https://developer.mozilla.org/en-US/docs/Web/CSS/element()) 的解释是，CSS 函数 `element()` 定义了一个从任意的 HTML 元素中生成的图像 `<image>` 值。该图像值是实时的，这意味着如果被指定的 HTML 元素被更改，应用了该属性的元素的背景也会相应更改。

它其实是个草案规范，但是一直以来，只有 Firefox 支持它 -- [CAN I USE -- CSS element()](https://caniuse.com/css-element-function)：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/4204c0fee5c24d769c395ab449f24908%7Etplv-k3u1fbpfcp-zoom-1.image)

它有什么作用呢？

### `-moz-element()` 如何使用

那么 `-moz-element()` 如何使用呢？简而言之，它能够复制一个元素内部渲染出来的 UI，并且能够实时同步变化。

假设我们有这样一个简单的结构，元素背景和内容都在运动：

``` html
<div id="bg" class="g-normal">
    <p>Content</p>
</div>
```

``` css
.g-normal {
    margin: auto;
    width: 200px;
    height: 200px;
    animation: change 5s infinite;
    background: linear-gradient(deeppink, yellowgreen);
}

p {
    animation: move 5s infinite;
}

@keyframes change {
    0% {
        filter: hue-rotate(0);
    }
    100% {
        filter: hue-rotate(360deg);
    }
}

@keyframes move {
    0% {
        transform: translate(0, 0);
    }
    100% {
        transform: translate(150px, 150px);
    }
}
```

它的效果大概是这样：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/123617162-8d486700-d839-11eb-9b0c-f090cbc52abd.gif)

我们就假设这个结构就是我们页面某一块的内容，然后，我们就可以使用 `background: -moz-element(#id)` 这种方式，将这个元素内绘制的 UI 内容完全拷贝至另外一个元素，看看效果。

我们添加一个元素 `<div class="g-element-copy"></div>`，在这个元素内模拟 `#bg` 内的内容：

``` html
<div id="bg" class="g-normal">
    <p>Content</p>
</div>
<div class="g-element-copy"></div>
```

``` css
.g-element-copy {
    margin: auto;
    width: 200px;
    height: 200px;
    // 核心代码
    background: -moz-element(#bg);
}
```

它可以完全复制另外一个元素内绘制出来的 UI，并且能追踪实时变化：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/123618303-a69de300-d83a-11eb-9ddd-4b6b929fcdac.gif)

[CodePen Demo -- -moz-element Demo(Firefox Only)](https://codepen.io/Chokcoco/pen/jOmOPPL)



## 在 firefox 中使用 element 复制 UI，用作毛玻璃元素背景

这样，有了上面的铺垫，下面的内容就比较好理解了。

和上述的 `background-attachment: fixed` 方案对比，我们还是通过伪元素叠加一层背景，只不过背景内的内容由单纯一张图片，变成了由 `-moz-element()` 复制的整段 UI 内容。

其次，上面的方案我们使用 `background-attachment: fixed` 使背景图和伪元素内叠加的图片的位置对齐，在这里，我们需要借助 Javascript 进行简单的运算，确定背景内容元素的相关位置，计算对齐量。

来看这样一个 DEMO：

``` html
<div id="bg" class="bg">
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
    <div>模拟真实 DOM</div>
</div>
<div class="g-glossy">frosted glass effect </div>
<div class="g-glossy-firefox"></div>
```

其中，`.g-glossy` 是在正常情况下 `backdrop-filter` 兼容时，我们的毛玻璃元素，而 `.g-glossy-firefox` 则是不兼容 `backdrop-filter` 时，我们需要模拟整个 DOM 背景 UI时候的元素，可以通过 CSS 特性检测 `CSS @support` 进行控制：

核心 CSS 代码：

``` css
.bg {
    // 整个页面的 DOM 结构
}

.g-glossy {
    position: fixed;
    width: 600px;
    height: 300px;
    background-color: rgba(255, 255, 255, 0.5);
    backdrop-filter: blur(10px);
}

.g-glossy-firefox {
    display: none;
}

@supports (background: -moz-element(#bg)) {
    .g-glossy-firefox {
        display: block;
        position: fixed;
        width: 600px;
        height: 300px;
        background: -moz-element(#bg) no-repeat;
        filter: blur(10px);
    }
}
```

简单解读一下：

1. 对于兼容 `backdrop-filter` 的，`.g-glossy` 内的代码将直接生效，并且 `.g-glossy-firefox` 不会展示
2. 对于 Firefox 浏览器，因为 `backdrop-filter` 必然不兼容，所以 `.g-glossy` 内的 `backdrop-filter: blur(10px)` 不会生效，而 `@supports (background: -moz-element(#bg))` 内的样式会生效，此时 `.g-glossy-firefox` 将会利用 `background: -moz-element(#bg) no-repeat;` 模拟 id 为 `bg` 的元素

当然，这里我们需要借助一定的 JavaScript 代码，计算我们的模拟页面 UI 的元素 `.g-glossy-firefox` 相对它模拟的 `#bg` 元素，也就是页面布局的一个定位偏差：

``` javascript
$(function() {
        let blur = $('.g-glossy-firefox')[0].style;
        let offset = $('.g-glossy').eq(0).offset();

        function updateBlur() {
            blur.backgroundPosition = 
                `${-window.scrollX - offset.left}px ` + 
                `${-window.scrollY - offset.top}px`;
        }
        document.addEventListener('scroll', updateBlur, false), updateBlur();
});
```

OK，至此，我们就能完美的在 Firefox 上也实现毛玻璃的效果了：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/123645631-14f19e00-d859-11eb-9aaa-3b0032da89e0.gif)

它相对于上面的第一种方案而言，最大的不同之处在于，它可以模拟各式各样的背景元素，背景元素可以不仅仅只是一张图片！它可以是各种复杂的结构！

这种方案是我的 CSS 群中，`风海流` 同学提供的一种思路，非常的巧妙，并且，他自己也对这种方案进行了完整的阐述，你可以戳这里看看：[在网页中实现标题栏「毛玻璃」效果](https://chaoli.club/index.php/5347)，本文也是经过他的同意，重新整理发出。

上述效果的完整代码，你可以戳这里：

[CodePen Demo -- 兼容 Firefox 的复杂背景毛玻璃（磨砂玻璃）效果](https://codepen.io/Chokcoco/pen/ExWqaQG)



## 总结一下

简单对上述内容进行一个总结：

- 你可以使用 `backdrop-filter` 对兼容它的浏览器非常简单的实现毛玻璃（磨砂玻璃）效果
- 对于不兼容 `backdrop-filter` 的浏览器，如果它只是简单背景，可以使用 `background-attachment: fixed` 配合 `filter: blur()` 进行模拟
- 对于 firefox 浏览器，你还可以使用 `moz-element()` 配合 `filter: blur()` 实现复杂背景毛玻璃效果
- 对于不兼容的上述 3 种效果的其他浏览器，设置了毛玻璃效果的元素，可以通过设置类似 `background: rgba(255, 255, 255, 0.5)` 的样式，使之回退到半透明效果，也算一种非常合理的降级效果，不会引起 Bug
