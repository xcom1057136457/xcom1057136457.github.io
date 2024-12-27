---
title: 仅使用CSS提高页面渲染速度
date: 2021-04-10 12:03:25
---
用户在访问一个Web网站（页面）或应用时，总是希望它的加载速度快，功能流畅。如果过于慢，用户就很有可能失去耐心而离开你的Web网站或应用。作为开发人员，给自己应用提供更快的访问速度，提供很好的用户体验是必备的基础技能，而且Web开发者在开发中也可以做很多事情来改善用户体验。那我们今天就来和大家聊聊，在CSS方面有哪些技巧可以帮助我们来提高Web页面的渲染速度。

## 内容可见性（content-visibility）

一般来说，大多数Web应用都有复杂的UI元素，而且有的内容会在设备可视区域之外（内容超出了用户浏览器可视区域），比如下图中红色区域就在手机设备屏幕可视区域之外：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/f7ff80b025bb4b5ba3ee8fa8f365b37c%7Etplv-k3u1fbpfcp-zoom-1.image)

在这种场合下，我们可以使用CSS的`content-visibility`来跳过屏幕外的内容渲染。也就是说，如果你有大量的离屏内容（Off-screen Content），这将会大幅减少页面渲染时间。

这个功能是CSS新增的特性，隶属于 [W3C 的 **CSS Containment Module Level 2** 模块](https://www.w3.org/TR/css-contain-2/#content-visibility)。也是对提高渲染性能影响最大的功能之一。`content-visibility`可以接受`visible`、`auto`和`hidden`三个属性值，但我们可以在一个元素上使用`content-visibility:auto`来直接的提升页面的渲染性能。

假设我们有一个像下面的页面，整个页面有个卡片列表，大约有`375`张，大约在屏幕可视区域能显示`12`张卡片。正如下图所示，渲染这个页面浏览器用时大约`1037ms`：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/e32f2b9a189d48f093ddc30b57cc8d32%7Etplv-k3u1fbpfcp-zoom-1.image)

你可以给所有卡片添加`content-visibility`：

``` css
.card {
    content-visibility: auto;
}
```

所有卡片加入`content-visibility`样式之后，页面的渲染时间下降到`150ms`，差不多提高了六倍的渲染性能：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/bdef585a84594ec69796c5d733d58500%7Etplv-k3u1fbpfcp-zoom-1.image)

正如你所看到的，`content-visibility`非常强大，提高页面渲染非常有用。换然话说，有了CSS的`content-visibility`属性，影响浏览器的渲染过程就变得更加容易。本质上，这个属性 **改变了一个元素的可见性，并管理其渲染状态**。

`content-visibility`有点类似于CSS的`display`和`visibility`属性，然而，`content-visibility`的操作方式与这些属性不同。

`content-visibility`的关键能力是，**它允许我们推迟我们选择的HTML元素渲染**。 默认情况之下，浏览器会渲染DOM树内所有可以被用户查看的元素。用户可以看到视窗可视区域中所有元素，并通过滚动查看页面内其他元素。一次渲染所有的元素（包括视窗可视区域之外不可见的HTML元素）可以让浏览器正确计算页面的尺寸，同时保持整个页面的布局和滚动条的一致性。

如果浏览器不渲染页面内的一些元素，滚动将是一场噩梦，因为无法正确计算页面高度。这是因为，`content-visibility`会将分配给它的元素的高度（`height`）视为`0`，浏览器在渲染之前会将这个元素的高度变为`0`，从而使我们的页面高度和滚动变得混乱。但如果已经为元素或其子元素显式设置了高度，那这种行为就会被覆盖。如果你的元素中没显式设置高度，并且因为显式设置`height`可能会带来一定的副作用而没设置，那么我们可以使用`contain-intrinsic-size`来确保元素的正确渲染，同时也保留延迟渲染的好处。

``` css
.card {
    content-visibility: auto;
    contain-intrinsic-size: 200px;
}
```

这也意味着它将像有一个“固有尺寸”（Intrinsic-size）的单一子元素一样布局，确保你没设置尺寸的`div`（示例中的`.card`）仍然占据空间。`contain-intrinsic-size`作为一个占位符尺寸来替代渲染内容。

虽然`contain-intrinsic-size`能让元素有一个占位空间，但如果有大量的元素都设置了`content-visibility: auto`，滚动条仍然会有较小的问题。

`content-visibility`提供的另外两个值`visible`和`hidden`可以让我们实现像元素的显式和隐藏，类似于`display`的`none`和非`none`值的切换：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/094d996900da45258b2d5d287b64a51b%7Etplv-k3u1fbpfcp-zoom-1.image)

在这种情况下，`content-visibility`可以提高频繁显示或隐藏的元素的渲染性能，例如模态框的显示和隐藏。`content-visibility`可以提供这种性能提升，这要归功于其隐藏值（`hidden`）的功能与其他值的不同：

- `display: none`：隐藏元素并破坏其渲染状态。 这意味着取消隐藏元素与渲染具有相同内容的新元素一样昂贵
- `visibility: hidden`：隐藏元素并保持其渲染状态。 这并不能真正从文档中删除该元素，因为它（及其子树）仍占据页面上的几何空间，并且仍然可以单击。 它也可以在需要时随时更新渲染状态，即使隐藏也是如此
- `content-visibility: hidden`：隐藏元素并保留其渲染状态。这意味着该元素隐藏时行为和`display: none`一样，但再次显示它的成本要低得多

`content-visibility`属性的扩展阅读：

- [`content-visibility`: the new CSS property that boosts your rendering performance](https://web.dev/content-visibility/)
- [More on `content-visibility`](https://css-tricks.com/more-on-content-visibility/)



## 合理使用will-change

[CSS渲染器（CSS Renderer）](https://blog.logrocket.com/how-browser-rendering-works-behind-the-scenes-6782b0e8fb10/)在渲染CSS样式之前需要一个准备过程，因为有些CSS属性需要CSS渲染器事先做很多准备才能实现渲染。这就很容易导致页面出现卡顿，给用户带来不好的体验。

比如Web上的动效，通常情况之下，Web动画（在动的元素）是和其他元素一起定期渲染的，以往在动画开发时，会使用CSS的3D变换（`transform`中的`translate3d()`或`translateZ()`）来开启GPU加速，让动画变得更流畅，但这样做是一种黑魔法，会将元素和它的上下文提到另一个“层”，独立于其他元素被渲染。可这种将元素提取到一个新层，相对来说代价也是昂贵的，这可能会使`transform`动画延迟几百毫秒。

不过，现在我可以不使用`transform`这样的Hack手段来开启GPU加速，可以直接使用CSS的`will-change`属性，该属性可以表明元素将修改特定的属性，让浏览器事先进行必要的优化。也就是说，`will-change`是一个UA提示，它不会对你使用它的元素产生任何样式上的影响。但值得注意的是，如果创建了新的层叠上下文，它可以产生外观效果。

比如下面这样的一个动画示例：

``` html
<!-- HTML -->
<div class="animate"></div>

/* CSS */
.animate {
    will-change: opacity
}
```

浏览器渲染上面的代码时，浏览器将为该元素创建一个单独的层。之后，它将该元素的渲染与其他优化一起委托给GPU，即，浏览器会识别`will-change`属性，并优化未来与不透明相关的变化。这将使动画变得更加流畅，因为GPU加速接管了动画的渲染。

> 根据 [@Maximillian Laumeister](https://www.maxlaumeister.com/articles/css-will-change-property-a-performance-case-study/) 所做的性能基准，可以看到，他通过这种单行变化获得了超过`120FPS`的渲染速度，和最初的渲染速度（大约`50FPS`）相比，提高`70FPS`左右。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/b0af9656ce0d4bf99d4e53b17852f022%7Etplv-k3u1fbpfcp-zoom-1.image)

`will-change`的使用并不复杂，它能接受的值有：

- `auto`：默认值，浏览器会根据具体情况，自行进行优化
- `scroll-position`：表示开发者将要改变元素的滚动位置，比如浏览器通常仅渲染可滚动元素“滚动窗口”中的内容。而某些内容超过该窗口（不在浏览器的可视区域内）。如果`will-change`显式设置了该值，将扩展渲染“滚动窗口”周围的内容，从而顺利地进行更长，更快的滚动（让元素的滚动更流畅）
- `content`：表示开发者将要改变元素的内容，比如浏览器常将大部分不经常改变的元素缓存下来。但如果一个元素的内容不断发生改变，那么产生和维护这个缓存就是在浪费时间。如果`will-change`显式设置了该值，可以减少浏览器对元素的缓存，或者完全避免缓存。变为从始至终都重新渲染元素。使用该值时需要尽量在文档树最末尾上使用，因为该值会被应用到它所声明元素的子节点，要是在文档树较高的节点上使用的话，可能会对页面性能造成较大的影响
- `<custom-ident>`：表示开发者将要改变的元素属性。如果给定的值是缩写，则默认被扩展全，比如，`will-change`设置的值是`padding`，那么会补全所有`padding`的属性，如 `will-change: padding-top, padding-right, padding-bottom, padding-left;`

详细的使用，请参阅：

- [CSS Will Change Module Level 1](https://www.w3.org/TR/css-will-change-1)
- [Everything You Need to Know About the CSS `will-change` Property](https://dev.opera.com/articles/css-will-change-property/)
- [CSS Reference：`will-change`](https://tympanus.net/codrops/css_reference/will-change/)

虽然说`will-change`能提高性能，但这个属性应该被认为是最后的手段，它不是为了过早的优化。只有消退你必须处理性能问题时，你才应该使用它。如果你滥用的话，反而会降低Web的性能。比如：

> **使用`will-change`表示该元素在未来会发生变化**。

因此，如果你试图将`will-change`和动画同时使用，它将不会给你带来优化。因此，建议在父元素上使用`will-change`，在子元素上使用动画。

``` css
.animate-element-parent {
    will-change: opacity;
}

.animate-element {
    transition: opacity .2s linear
}
```

> **不要使用非动画元素**。

当你在一个元素上使用`will-change`时，浏览器会尝试通过将元素移动到一个新的图层并将转换工作交互GPU来优化它。如果你没有任何要转换的内容，则会导致资源浪费。

除此之外，要用好`will-change`也不是件易事，[MDN在这方面做出了相应的描述](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change)：

- **不要将 `will-change` 应用到太多元素上**：浏览器已经尽力尝试去优化一切可以优化的东西了。有一些更强力的优化，如果与 `will-change` 结合在一起的话，有可能会消耗很多机器资源，如果过度使用的话，可能导致页面响应缓慢或者消耗非常多的资源。比如 `*{will-change: transform, opacity;}`
- **有节制地使用**：通常，当元素恢复到初始状态时，浏览器会丢弃掉之前做的优化工作。但是如果直接在样式表中显式声明了 `will-change` 属性，则表示目标元素可能会经常变化，浏览器会将优化工作保存得比之前更久。所以最佳实践是当元素变化之前和之后通过脚本来切换 `will-change` 的值
- **不要过早应用 `will-change` 优化**：如果你的页面在性能方面没什么问题，则不要添加 `will-change` 属性来榨取一丁点的速度。 `will-change` 的设计初衷是作为最后的优化手段，用来尝试解决现有的性能问题。它不应该被用来预防性能问题。过度使用 `will-change` 会导致大量的内存占用，并会导致更复杂的渲染过程，因为浏览器会试图准备可能存在的变化过程。这会导致更严重的性能问题。
- **给它足够的工作时间**：这个属性是用来让页面开发者告知浏览器哪些属性可能会变化的。然后浏览器可以选择在变化发生前提前去做一些优化工作。所以给浏览器一点时间去真正做这些优化工作是非常重要的。使用时需要尝试去找到一些方法提前一定时间获知元素可能发生的变化，然后为它加上 `will-change` 属性。

最后需要注意的是，建议在完成所有动画后，将元素的`will-change`删除。下面这个示例展示如何使用脚本正确地应用 `will-change` 属性的示例，在大部分的场景中，你都应该这样做。

``` javascript
var el = document.getElementById('element');

// 当鼠标移动到该元素上时给该元素设置 will-change 属性
el.addEventListener('mouseenter', hintBrowser);
// 当 CSS 动画结束后清除 will-change 属性
el.addEventListener('animationEnd', removeHint);

function hintBrowser() {
    // 填写上那些你知道的，会在 CSS 动画中发生改变的 CSS 属性名们
    this.style.willChange = 'transform, opacity';
}

function removeHint() {
    this.style.willChange = 'auto';
}
```

在实际使用`will-change`可以记作以下几个规则，即 **五可做，三不可做**：

- 在样式表中少用`will-change`
- 给`will-change`足够的时间令其发挥该有的作用
- 使用`<custom-ident>`来针对超特定的变化（如，`left, opacity`等）
- 如果需要的话，可以JavaScript中使用它（添加和删除）
- 修改完成后，删除`will-change`
- 不要同时声明太多的属性
- 不要应用在太多元素上
- 不要把资源浪费在已停止变化的元素上

## 让元素及其内容尽可能独立于文档树的其余部分（contain）

[**W3C的CSS Containment Module Level 2**](https://www.w3.org/TR/css-contain-2/#contain-property)除了提供前面介绍的`content-visibility`属性之外，还有另一个属性`contain`。该属性允许我们指定特定的DOM元素和它的子元素，让它们能够独立于整个DOM树结构之外。目的是能够让浏览器有能力只对部分元素进行重绘、重排，而不必每次针对整个页面。即，允许浏览器针对DOM的有限区域而不是整个页面重新计算布局，样式，绘画，大小或它们的任意组合。

在实际使用的时候，我们可以通过`contain`设置下面五个值中的某一个来规定元素以何种方式独立于文档树：

- **`layout`** ：该值表示元素的内部布局不受外部的任何影响，同时该元素以及其内容也不会影响以上级
- **`paint`** ：该值表示元素的子级不能在该元素的范围外显示，该元素不会有任何内容溢出（或者即使溢出了，也不会被显示）
- **`size`** ：该值表示元素盒子的大小是独立于其内容，也就是说在计算该元素盒子大小的时候是会忽略其子元素
- **`content`** ：该值是`contain: layout paint`的简写
- **`strict`** ：该值是`contain: layout paint size`的简写

在上述这几个值中，`size`、`layout`和`paint`可以单独使用，也可以相互组合使用；另外`content`和`strict`是组合值，即`content`是`layout paint`的组合，`strict`是`layout paint size`的组合。

`contain`的`size`、`layout`和`paint`提供了不同的方式来影响浏览器渲染计算：

- `size`： 告诉浏览器，当其内容发生变化时，该容器不应导致页面上的位置移动
- `layout`：告诉浏览器，容器的后代不应该导致其容器外元素的布局改变，反之亦然
- `paint`：告诉浏览器，容器的内容将永远不会绘制超出容器的尺寸，如果容器是模糊的，那么就根本不会绘制内容

[@Manuel Rego Casasnovas提供了一个示例](https://blogs.igalia.com/mrego/files/2019/01/css-contain-example.html)，向大家[阐述和演示了`contain`是如何提高Web页面渲染性能](https://blogs.igalia.com/mrego/2019/01/11/an-introduction-to-css-containment/?ref=heydesigner)。这个示例中，有`10000`个像下面这样的DOM元素：

``` html
<div class="item">
    <div>Lorem ipsum...</div>
</div>
```

使用JavaScript的`textContent`这个API来动态更改`div.item > div`的内容：

``` javascript
const NUM_ITEMS = 10000;
const NUM_REPETITIONS = 10;

function log(text) {
    let log = document.getElementById("log");
    log.textContent += text;
}

function changeTargetContent() {
    log("Change \"targetInner\" content...");

    // Force layout.
    document.body.offsetLeft;

    let start = window.performance.now();

    let targetInner = document.getElementById("targetInner");
    targetInner.textContent = targetInner.textContent == "Hello World!" ? "BYE" : "Hello World!";

    // Force layout.
    document.body.offsetLeft;

    let end = window.performance.now();
    let time = window.performance.now() - start;
    log(" Time (ms): " + time + "\n");
    return time;
}

function setup() {
    for (let i = 0; i < NUM_ITEMS; i++) {
        let item = document.createElement("div");
        item.classList.add("item");

        let inner = document.createElement("div");
        inner.style.backgroundColor = "#" + Math.random().toString(16).slice(-6);
        inner.textContent = "Lorem ipsum...";
        item.appendChild(inner);

        wrapper.appendChild(item);
    }
}
```

如果不使用`contain`，即使更改是在单个元素上，浏览器在布局上的渲染也会花费大量的时间，因为它会遍历整个DOM树（在本例中，DOM树很大，因为它有`10000`个DOM元素）：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/8dd837225d5b46a996301e0ab9c36071%7Etplv-k3u1fbpfcp-zoom-1.image)

在本例中，`div`的大小是固定的，我们在内部`div`中更改的内容不会溢出它。因此，我们可以将`contain: strict`应用到项目上，这样当项目内部发生变化时，浏览器就不需要访问其他节点，它可以停止检查该元素上的内容，并避免到外部去。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/04936a6174c84ffa9b97d6dc568366b6%7Etplv-k3u1fbpfcp-zoom-1.image)

尽管这个例子中的每一项都很简单，但通过使用`contain`，Web性能得到很大的改变，从`~4ms`降到了`~0.04ms`，这是一个巨大的差异。想象一下，如果DOM树具有非常复杂的结构和内容，但只修改了页面的一小部分，如果可以将其与页面的其他部分隔离开来，那么将会发生什么情况呢？

有关于`contain`的更多内容：

- [Let’s Take a Deep Dive Into the CSS Contain Property](https://css-tricks.com/lets-take-a-deep-dive-into-the-css-contain-property/)
- [Helping Browsers Optimize With The CSS Contain Property](https://www.smashingmagazine.com/2019/12/browsers-containment-css-contain-property/)
- [CSS `contain` Property](https://termvader.github.io/css-contain/)

## 使用font-display解决由于字体造成的布局偏移（FOUT）

在Web开发的过程中，难免会使用`@font-face`技术引用一些特殊字体（系统没有的字体），同时也可能会配合变量字体特性，使用更具个性化的字体。

使用`@font-face`加载字体策略大概如下图所示：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/08409c6aae0148a8b3b5214e8e766e1d%7Etplv-k3u1fbpfcp-zoom-1.image)

上图来自于[@zachleat](https://twitter.com/zachleat)的《[A COMPREHENSIVE GUIDE TO FONT LOADING STRATEGIES](https://www.zachleat.com/web/comprehensive-webfonts/)》一文。

Web中使用非系统字体（`@font-face`规则引入的字体）时，浏览器可能没有及时得到Web字体，就会让它用后备系统字体渲染，然后优化我们的字体。这个时候很容易引起未编排(Unstyled)的文本引起闪烁，整个排版本布局也看上去会偏移一下（FOUT）。

幸运的是，根据`@font-face`规则，`font-display`属性定义了浏览器如何加载和显示字体文件，允许文本在字体加载或加载失败时显示回退字体。可以通过依靠折中无样式文本闪现使文本可见替代白屏来提高性能。

CSS的`font-display`属性有五个不同的值：

- **`auto`** ：默认值。典型的浏览器字体加载的行为会发生，也就是使用自定义字体的文本会先被隐藏，直到字体加载结束才会显示。即字体展示策略与浏览器一致，当前，大多数浏览器的默认策略类似`block`
- **`block`** ：给予字体一个较短的阻塞时间（大多数情况下推荐使用 `3s`）和无限大的交换时间。换言之，如果字体未加载完成，浏览器将首先绘制“隐形”文本；一旦字体加载完成，立即切换字体。为此，浏览器将创建一个匿名字体，其类型与所选字体相似，但所有字形都不含“墨水”。使用特定字体渲染文本之后页面方才可用，只有这种情况下才应该使用 `block`。
- **`swap`** ：使用 `swap`，则阻塞阶段时间为 `0`，交换阶段时间无限大。也就是说，如果字体没有完成加载，浏览器会立即绘制文字，一旦字体加载成功，立即切换字体。与 block 类似，如果使用特定字体渲染文本对页面很重要，且使用其他字体渲染仍将显示正确的信息，才应使用 `swap`。
- **`fallback`** ：这个可以说是`auto`和`swap`的一种折中方式。需要使用自定义字体渲染的文本会在较短的时间不可见，如果自定义字体还没有加载结束，那么就先加载无样式的文本。一旦自定义字体加载结束，那么文本就会被正确赋予样式。使用 `fallback`时，阻塞阶段时间将非常小（多数情况下推荐小于 `100ms`），交换阶段也比较短（多数情况下建议使用 `3s`）。换言之，如果字体没有加载，则首先会使用后备字体渲染。一旦加载成功，就会切换字体。但如果等待时间过久，则页面将一直使用后备字体。如果希望用户尽快开始阅读，而且不因新字体的载入导致文本样式发生变动而干扰用户体验，`fallback` 是一个很好的选择。
- **`optional`** ：效果和`fallback`几乎一样，都是先在极短的时间内文本不可见，然后再加载无样式的文本。不过`optional`选项可以让浏览器自由决定是否使用自定义字体，而这个决定很大程度上取决于浏览器的连接速度。如果速度很慢，那你的自定义字体可能就不会被使用。使用 `optional` 时，阻塞阶段时间会非常小（多数情况下建议低于 `100ms`），交换阶段时间为 `0`。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/4166c647799943c4a9169db81ace759d%7Etplv-k3u1fbpfcp-zoom-1.image)

下面是使用`swap`值的一个例子：

``` css
@font-face {
    font-family: "Open Sans Regular";
    font-weight: 400;
    font-style: normal;
    src: url("fonts/OpenSans-Regular-BasicLatin.woff2") format("woff2");
    font-display: swap;
}
```

在这个例子里我们通过只使用`WOFF2`文件来缩写字体。另外我们使用了`swap`作为`font-display`的值，页面的加载情况将如下图所示：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/2a9442c1c9a6415e8e61192d0613e3d7%7Etplv-k3u1fbpfcp-zoom-1.image)

> **注意，`font-display`一般放在`@font-face`规则中使用**。

有关于字体加载和`font-display`更多的介绍，可以阅读：

- [A deep dive into webfonts](https://iamschulz.com/a-deep-dive-into-webfonts/)
- [How to avoid layout shifts caused by web fonts](https://simonhearne.com/2021/layout-shifts-webfonts/)
- [The Best Font Loading Strategies and How to Execute Them](https://css-tricks.com/the-best-font-loading-strategies-and-how-to-execute-them/)
- [A font-display setting for slow connections](https://calendar.perfplanet.com/2020/a-font-display-setting-for-slow-connections/)
- [How to Load Fonts in a Way That Fights FOUT and Makes Lighthouse Happy](https://css-tricks.com/how-to-load-fonts-in-a-way-that-fights-fout-and-makes-lighthouse-happy/)
- [The importance of `@font-face` source order when used with preload](https://nooshu.github.io/blog/2021/01/23/the-importance-of-font-face-source-order-when-used-with-preload/)
- [The Fastest Google Fonts](https://csswizardry.com/2020/05/the-fastest-google-fonts/)
- [A COMPREHENSIVE GUIDE TO FONT LOADING STRATEGIES](https://www.zachleat.com/web/comprehensive-webfonts/)

## scroll-behavior让滚动更流畅

早前在滚动的特性和改变用户体验的滚动新特性中向大家介绍了几个可以用来改变用户体验的滚动特性，比如滚动捕捉、`overscroll-behavior`和`scroll-behavior`。

[`scroll-behavior`是CSSOM View Module提供的一个新特性](https://dev.w3.org/csswg/cssom-view/)，可以轻易的帮助我们实现丝滑般的滚动效果。该属性可以为一个滚动框指定滚动行为，其他任何的滚动，例如那些由于用户行为而产生的滚动，不受这个属性的影响。

`scroll-behavior`接受两个值：

- **`auto`** ：滚动框立即滚动
- **`smooth`** ：滚动框通过一个用户代理定义的时间段使用定义的时间函数来实现平稳的滚动，用户代理平台应遵循约定，如果有的话

除此之外，其还有三个全局的值：`inherit`、`initial`和`unset`。

使用起来很简单，只需要这个元素上使用`scroll-behavior:smooth`。因此，很多时候为了让页面滚动更平滑，建议在`html`中直接这样设置一个样式：

``` css
html {
    scroll-behavior:smooth;
}
```

口说无凭，来看个效果对比，你会有更好的感觉：

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/e963921433704c3386553e9e4d8c97e6%7Etplv-k3u1fbpfcp-zoom-1.image)

有关于`scroll-behavior`属性更多的介绍可以再花点时间阅读下面这些文章：

- [CSSOM View Module:`scroll-behavior`](https://www.w3.org/TR/cssom-view-1/#smooth-scrolling)
- [CSS-Tricks: `scroll-behavior`](https://css-tricks.com/almanac/properties/s/scroll-behavior/)
- [Native Smooth Scroll behavior](https://hospodarets.com/native_smooth_scrolling)
- [PAGE SCROLLING IN VANILLA JAVASCRIPT](https://pawelgrzybek.com/page-scroll-in-vanilla-javascript/)
- [smooth scroll behavior polyfill](https://iamdustan.com/smoothscroll/)

## 开启GPU渲染动画

浏览器针对处理CSS动画和不会很好地触发重排（因此也导致绘）的动画属性进行了优化。为了提高性能，可以将被动画化的节点从主线程移到GPU上。将导致合成的属性包括 3D transforms (`transform: translateZ()`, `rotate3d()`，等)，`animating`， `transform` 和 `opacity`, `position: fixed`，`will-change`，和 `filter`。一些元素，例如 `<video>`, `<canvas>` 和 `<iframe>`，也位于各自的图层上。 将元素提升为图层（也称为合成）时，动画转换属性将在GPU中完成，从而改善性能，尤其是在移动设备上。

## 减少渲染阻止时间

今天，许多Web应用必须满足多种形式的需求，包括PC、平板电脑和手机等。为了完成这种响应式的特性，我们必须根据媒体尺寸编写新的样式。当涉及页面渲染时，它无法启动渲染阶段，直到 CSS对象模型（CSSOM）已准备就绪。根据你的Web应用，你可能会有一个大的样式表来满足所有设备的形式因素。

但是，假设我们根据表单因素将其拆分为多个样式表。在这种情况下，我们可以只让主CSS文件阻塞关键路径，并以高优先级下载它，而让其他样式表以低优先级方式下载。

``` html
<link rel="stylesheet" href="styles.css">
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/8330734e4f4c4f6ebda8d80f0dd0f0c6%7Etplv-k3u1fbpfcp-zoom-1.image)

将其分解为多个样式表后：

``` html
<!-- style.css contains only the minimal styles needed for the page rendering -->
<link rel="stylesheet" href="styles.css" media="all" />

<!-- Following stylesheets have only the styles necessary for the form factor -->
<link rel="stylesheet" href="sm.css" media="(min-width: 20em)" />
<link rel="stylesheet" href="md.css" media="(min-width: 64em)" />
<link rel="stylesheet" href="lg.css" media="(min-width: 90em)" />
<link rel="stylesheet" href="ex.css" media="(min-width: 120em)" />
<link rel="stylesheet" href="print.css" media="print" />
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/3e1a7c958f564bf2970e20495e9ce539%7Etplv-k3u1fbpfcp-zoom-1.image)

默认情况下，浏览器假设每个指定的样式表都是阻塞渲染的。通过添加 `media`属性附加媒体查询，告诉浏览器何时应用样式表。当浏览器看到一个它知道只会用于特定场景的样式表时，它仍会下载样式，但不会阻塞渲染。通过将 CSS 分成多个文件，主要的 阻塞渲染 文件（本例中为 `styles.css`）的大小变得更小，从而减少了渲染被阻塞的时间。

## 避免@import包含多个样式表

通过 `@import`，我们可以在另一个样式表中包含一个样式表。当我们在处理一个大型项目时，使用 `@import` 可以使代码更加简洁。

关于 `@import` 的关键事实是，它是一个阻塞调用，因为它必须通过网络请求来获取文件，解析文件，并将其包含在样式表中。如果我们在样式表中嵌套了 `@import`，就会妨碍渲染性能。

``` css
/* style.css */
@import url("windows.css");

/* windows.css */
@import url("componenets.css");
```

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/597792ef5ff140f09057a041e714dc79%7Etplv-k3u1fbpfcp-zoom-1.image)

与使用 `@import` 相比，我们可以通过多个 `link` 来实现同样的功能，但性能要好得多，因为它允许我们并行加载样式表。

![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/ad86fb5764a249cabe15908fb0d46bb6%7Etplv-k3u1fbpfcp-zoom-1.image)

## 注意动态修改自定义属性方式

CSS自定义属性又名CSS变量，该特性已经是非常成熟的特性了，可以在Web的开发中大胆的使用该特性：

``` css
:root { --color: red; }

button {
    color: var(--color);
}
```

在使用CSS自定义属性时，时常在`root`（根元素）上注册自定义属性，这种方式注册的自定义属性是个全局的自定义属性（全局变量），可以被所有嵌套的子元素继承。就上例而言，`--color`属性允许任何`button`样式将其作为变量使用。

熟悉CSS自定义属性的同学都知道，可以使用`style.setProperty`来重新设置已注册好的自定义属性的值。但在修改根自定义属性时，需要注意，因为它会影响Web的性能。早在2017年@Lisi Linhart 在《[Performance of CSS Variables](https://lisilinhart.info/posts/css-variables-performance/)》中阐述过。

* 在使用CSS变量时，我们总是要注意我们的变量是在哪个范围内定义的，如果改变它，将影响许多子代，从而产生大量的样式重新计算。

* 结合CSS变量使用`calc()`是一个很好的方法，可以获得更多的灵活性，限制我们需要定义的变量数量。在不同的浏览器中测试`calc()`与CSS变量的结合，并没有发现任何大的性能问题。然而在一些浏览器中对一些单位的支持还是有限的，比如`deg`或`ms`，所以我们必须记住这一点。

* 如果我们比较一下在JavaScript中通过内联样式设置变量与`setProperty`方法的性能标志，浏览器之间有一些明显的差异。在Safari中通过内联样式设置属性的速度非常快，而在Firefox中则非常慢，所以使用`setProperty`设置变量是首选

有关于这方面的具体细节就不在这阐述了，如果你对这方面感兴趣的话，可以阅读下面这几篇文章：

- [Performance of CSS Variables](https://lisilinhart.info/posts/css-variables-performance/)
- [Control CSS loading with custom properties](https://jakearchibald.com/2016/css-loading-with-custom-props/)
- [CSS Custom Properties performance in 2018](https://blog.jiayihu.net/css-custom-properties-performance-in-2018/)
- [Improving CSS Custom Properties performance](https://blogs.igalia.com/jfernandez/2020/08/13/improving-css-custom-properties-performance/)

## 小结

可能很多人会说，5G已到来，终端设备性能越来越好，网络环境也越来越强，Web性能已不是问题了，但事实上在Web开发过程中总是难免碰到性能是的问题。而且我们为用户提供更流畅的体验也是我们必备技术之一。时至今日，优化Web性能的方式和手段很多，但在开发时注重每个细节，可以让我们把性能做得更好。正如文章中提到这些。

除了文章提到的这几个点，还有一些其他的方法可以使用CSS来提高网页的性能。当然，文章中提到的一些特性还没有得到所有浏览器支持，比如`content-visibility`、`contain`等，但在未来它们肯定能让页面渲染带来更快的渲染。另外，文章中提到的一些技巧并没有深入阐述，比如CSS的引用方式，CSS的阻塞等。

