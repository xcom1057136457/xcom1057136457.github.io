---
title: '你真的熟练运用HTML5了吗,这10个酷炫的H5特性你会几个？'
date: 2021-07-17 16:36:27
---
HTML5不是什么新鲜事。自初始版本（2008 年 1 月）以来，我们一直在使用它的几个功能。我再次仔细查看了 HTML5 功能列表。看看我发现了什么？到目前为止，我还没有真正使用过很多！

在本文中，我列出了 10 个这样的HTML5功能，这些功能过去我用得不多，但现在发现它们很有用。我还创建了一个工作示例流程并托管在GitHub. 希望你也觉得它有用。让我们开始了解有关它们中的每一个的解释、代码和快速提示。



# 🍖 一、详情标签

该`<details>`标签向用户提供按需详细信息。如果您需要按需向用户显示内容，请使用此标签。默认情况下，小部件是关闭的。打开时，它会展开并显示其中的内容。

该`<summary>`标签用于`<details>`为它指定一个可见的标题。

**代码**

``` html
<details>
    <summary>Click Here to get the user details</summary>
    <table>
        <tr>
            <th>#</th>
            <th>Name</th>
            <th>Location</th>
            <th>Job</th>
        </tr>
        <tr>
            <td>1</td>
            <td>Adam</td>
            <td>Huston</td>
            <td>UI/UX</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Bob</td>
            <td>London</td>
            <td>Machine Learning</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Jack</td>
            <td>Australia</td>
            <td>UI Designer</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Tapas</td>
            <td>India</td>
            <td>Blogger</td>
        </tr>
    </table>
</details>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dd422b1856684055899b77d50233f055~tplv-k3u1fbpfcp-zoom-1.image)



# 🎶 二、内容可编辑

contenteditable是可以在元素上设置以使内容可编辑的属性。它适用于 DIV、P、UL 等元素。您必须指定它，例如，`<element contenteditable="true|false">`。

**==注意==: 当contenteditable元素上没有设置属性时，它将从其父元素继承。**

**代码**

``` html
<h2> Shoppping List(Content Editable) </h2>
 <ul class="content-editable" contenteditable="true">
     <li> 1. Milk </li>
     <li> 2. Bread </li>
     <li> 3. Honey </li>
</ul>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/13d54103f89d427e819b1b0a6f32a2b7~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**span 或 div 元素可以使用它进行编辑，您可以使用 CSS 样式向其中添加任何丰富的内容。这将比使用输入字段处理它要好得多。去试一试！**



# ✨ 三、地图

该`<map>`标签有助于定义图像映射。图像映射是其中包含一个或多个可点击区域的图像。地图标签带有一个`<area>`标签来确定可点击区域。可点击区域可以是这些形状、矩形、圆形或多边形区域之一。如果您不指定任何形状，它会考虑整个图像。

**代码**

``` html
<div>
    <img src="circus.jpg" width="500" height="500" alt="Circus" usemap="#circusmap">

    <map name="circusmap">
        <area shape="rect" coords="67,114,207,254" href="elephant.htm">
        <area shape="rect" coords="222,141,318, 256" href="lion.htm">
        <area shape="rect" coords="343,111,455, 267" href="horse.htm">
        <area shape="rect" coords="35,328,143,500" href="clown.htm">
        <area shape="circle" coords="426,409,100" href="clown.htm">
    </map>
 </div>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ebeb350635874d75a7f4ebf15e0db9c3~tplv-k3u1fbpfcp-watermark.image)

==**快速提示**==

**图像地图有其自身的缺点，但您可以将其用于视觉演示。试试看一张全家福怎么样，然后深入到个人的照片（可以是我们一直珍视的旧照片！）。**



# 🏀 四、标记内容

使用`<mark>`标签突出显示任何文本内容。

```
<p> 你知道吗，你可以仅使用 HTML 标签 <mark>"突出显示有趣的东西"</mark></p>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/19b62d9d581645f9a778c3c80dd86131~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**您可以随时使用 css 更改高亮颜色**

``` css
mark {
  background-color: green;
  color: #FFFFFF;
}
```



# 🎥 五、data-* 属性

这些`data-*`属性用于存储页面或应用程序私有的自定义数据。存储的数据可用于 JavaScript 代码以创建进一步的用户体验。

`data-*` 属性由两部分组成：

- **属性名称不应包含任何大写字母，并且必须在前缀“data-”之后至少长一个字符**
- **属性值可以是任何字符串**

**代码**

``` html
<h2> Know data attribute </h2>
 <div 
       class="data-attribute" 
       id="data-attr" 
       data-custom-attr="You are just Awesome!"> 
   I have a hidden secret!
  </div>

 <button onclick="reveal()">Reveal</button>
```

然后在 JavaScript 中，

``` javascript
function reveal() {
   let dataDiv = document.getElementById('data-attr');
    let value = dataDiv.dataset['customAttr'];
   document.getElementById('msg').innerHTML = `<mark>${value}</mark>`;
}
```

注意：要在 JavaScript 中读取这些属性的值，您可以使用`getAttribute()`它们的完整 HTML 名称（即 data-custom-attr），但标准定义了一种更简单的方法：使用`dataset`属性。

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a319e3c2d5174b079b519fca8d590559~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**您可以使用它在页面上存储一些数据，然后使用 REST 调用将其传递给服务器。**



# 🏆 六、输出标签

`<output>`标签表示的运算的结果。通常，此元素定义将用于显示某些计算的文本输出的区域。

**代码**

``` html
<form oninput="x.value=parseInt(a.value) * parseInt(b.value)">
   <input type="number" id="a" value="0">
          * <input type="number" id="b" value="0">
                = <output name="x" for="a b"></output>
</form>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/216d619a8e0b4052ab55089ac531820d~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**如果您在客户端 JavaScript 中执行任何计算，并且希望结果反映在页面上，请使用`<output>`标记。您不必执行使用 获取元素的额外步骤getElementById()。**



# 🎻 七、数据列表

`<datalist>`标签指定了一个预定义选项列表，并允许用户向其中添加更多选项。它提供了一项autocomplete功能，允许您通过预先输入获得所需的选项。

**代码**

``` html
<form action="" method="get">
    <label for="fruit">Choose your fruit from the list:</label>
    <input list="fruits" name="fruit" id="fruit">
        <datalist id="fruits">
           <option value="Apple">
           <option value="Orange">
           <option value="Banana">
           <option value="Mango">
           <option value="Avacado">
        </datalist>
     <input type="submit">
 </form>  
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d250285d3dcb49c5be476f4e50f94562~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**它与传统`<select>-<option>`标签有何不同？选择标签用于从您需要浏览列表的选项中选择一项或多项。Datalist是具有自动完成支持的高级功能。**



# 🧿 八、范围（滑块）

`range`是给定滑块类型范围选择器的输入类型。

**代码**

``` html
<form method="post">
    <input 
         type="range" 
         name="range" 
         min="0" 
         max="100" 
         step="1" 
         value=""
         onchange="changeValue(event)"/>
 </form>
 <div class="range">
      <output id="output" name="result">  </output>
 </div>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff9208cca3a3482e919c613d5af20fb4~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**HTML5 中没有叫slider的！**



# ⏰ 九、Meter

使用`<meter>`标签测量给定范围内的数据。

**代码**

``` html
<label for="home">/home/atapas</label>
<meter id="home" value="4" min="0" max="10">2 out of 10</meter><br>

<label for="root">/root</label>
<meter id="root" value="0.6">60%</meter><br>
```

**看看它如何工作**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e84130686b964b10ad20d737424b1e2e~tplv-k3u1fbpfcp-zoom-1.image)

==**快速提示**==

**不要将`<meter>`标签用于进度指示器类型的用户体验。我们有来自 HTML5的`<Progress>`标签。**

``` html
<label for="file">Downloading progress:</label>
<progress id="file" value="32" max="100"> 32% </progress>
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7adeb88e92e41a488c662e34d858f8e~tplv-k3u1fbpfcp-zoom-1.image)



# 💌 十、Inputs

这部分是我们最熟悉的输入类型的用法，如文本、密码等。输入类型的特殊用法很少

**代码**

必需的 将输入字段标记为必填字段。

``` html
<input type="text" id="username1" name="username" required>
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/522023617b7f4171ab20774ae214dc42~tplv-k3u1fbpfcp-zoom-1.image)

自动对焦 通过将光标放在输入元素上自动提供焦点。

``` html
<input type="text" id="username2" name="username" required autofocus>
```

使用正则表达式验证 您可以使用正则表达式指定模式来验证输入。

``` html
<input type="password" 
       name="password" 
       id="password" 
       placeholder="6-20 chars, at least 1 digit, 1 uppercase and one lowercase letter" 
       pattern="^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{6,20}$" autofocus required>
```

颜色选择器 一个简单的颜色选择器。

``` html
<input type="color" onchange="showColor(event)">
<p id="colorMe">Color Me!</p>
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e0d39d4f37e4440993c4f9610932145~tplv-k3u1fbpfcp-zoom-1.image)

