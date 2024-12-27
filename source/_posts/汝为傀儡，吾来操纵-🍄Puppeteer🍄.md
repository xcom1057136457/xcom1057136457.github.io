---
title: 汝为傀儡，吾来操纵(🍄Puppeteer🍄)
date: 2024-12-27 19:59:21
---
puppeteer是我以前同事使用过的一个工具，用来测试页面的功能，可以模拟用户操作。这几天我也看到了一些相关的文章，也是很感兴趣的，所以准备整理输出一篇Puppeteer的文章，用来学习记忆。

后面发现Puppeteer相关的内容比较多，准备分为两部分来讲，这一部分主要讲理论相关的，也会举些简单的实例。下一章则会主要针对实战来讲解。

## 介绍

Puppeteer词义解释

* Puppet：木偶，傀儡
* Puppeteer：操纵木偶的人

> Puppeteer 是一个由 Google 开发的 Node.js 库，用于控制 Chrome 或 Chromium 浏览器的高级 API。它可以模拟用户的交互行为，例如点击、填写表单、导航等，同时还可以截取页面内容、生成 PDF、执行自动化测试等功能。

官方网站：[https://github.com/GoogleChrome/puppeteer](https://github.com/GoogleChrome/puppeteer)

[Puppeteer 中文文档](https://puppeteer.bootcss.com/)

官方文档：[https://pptr.dev/](https://pptr.dev/)

文档地址：[https://zhaoqize.github.io/puppeteer-api-zh_CN/#](https://zhaoqize.github.io/puppeteer-api-zh_CN/#)

## 核心功能

Puppeteer 的核心功能包括以下几个方面：

1. **控制浏览器：** Puppeteer 可以启动一个 Chrome 或 Chromium 浏览器实例，并通过 API 控制浏览器的行为，如打开网页、点击链接、填写表单、执行 JavaScript 等操作。

2. **页面操作：** Puppeteer 可以模拟用户在页面上的操作，包括点击元素、填写表单、滚动页面、截取屏幕截图等，实现对页面的交互操作。

3. **网页内容抓取：** Puppeteer 可以获取页面的 DOM 结构、元素属性、文本内容等信息，从而实现网页内容的抓取和提取。

4. **页面性能分析：** Puppeteer 可以获取页面加载性能数据、网络请求信息、CPU 和内存使用情况等，帮助开发者进行页面性能优化和调试。

5. **生成 PDF：** Puppeteer 可以将网页内容保存为 PDF 文档，支持设置页面大小、方向、页边距等参数，方便生成打印版的网页内容。

6. **自动化测试：** Puppeteer 可以用于编写自动化测试脚本，模拟用户的操作行为，验证页面的功能和交互是否符合预期，实现自动化测试流程。

7. **爬虫和数据采集：** Puppeteer 可以用于编写网络爬虫，自动访问网页、提取数据、填写表单等，实现网页内容的自动采集和处理。

总的来说，Puppeteer 是一个功能强大的浏览器自动化工具，可以实现对浏览器的控制和页面操作，适用于各种场景下的自动化任务，如自动化测试、网页内容抓取、页面性能分析等。

下面介绍一些常用的API。

## 启动新的浏览器实例

> puppeteer.launch()是一个用于启动一个新的浏览器实例的方法。该方法返回一个 Promise，该 Promise 在浏览器实例启动后会被解析为一个 Browser 对象，你可以通过这个对象来操作浏览器

在 Puppeteer 中，puppeteer.launch() 方法可以接受一个可选的配置对象 options，用于指定启动浏览器实例时的一些参数和选项。下面是一些常用的配置选项：

1. **headless：** 布尔值，是否以 无头模式 运行浏览器。默认是 true，即以无头模式启动，不会显示浏览器界面。如果设置为 false，则会以有头模式启动，显示浏览器界面。

2. **args：** 一个字符串数组，传递给浏览器实例的其他参数。 这些参数可以参考[这里](http://peter.sh/experiments/chromium-command-line-switches/)。

3. defaultViewport 是一个对象，用于为每个页面设置一个默认视口大小。默认是 800x600。如果为 null 的话就禁用视图口。下面是 defaultViewport 对象中可以设置的属性：

   * width：页面的宽度像素。
   
   * height：页面的高度像素。
   
   * deviceScaleFactor：设备的缩放比例，可以认为是设备像素比（device pixel ratio，DPR）。默认值为 1。
   
   * [更多](https://puppeteer.bootcss.com/api#puppeteerlaunchoptions)

4. **ignoreHTTPSErrors:** 布尔值，指定是否忽略 HTTPS 错误。默认是 false。

5. **defaultViewport：** 一个对象，用于指定浏览器的默认视口大小，包括宽度、高度和设备比例因子等。

6. **userDataDir：** 一个字符串，用于指定用户数据目录的路径，用于存储浏览器的用户数据，比如缓存、Cookies 等。

7. **timeout:** 数值，指定启动浏览器的超时时间，单位为毫秒。

8. **slowMo:** 数值，指定 Puppeteer 操作的延迟时间，单位为毫秒。可以用来减慢操作的速度，方便调试。

下面是一个简单的示例代码，演示如何使用 puppeteer.launch() 方法来启动一个浏览器实例：

```` javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({
    headless: false, // 显示浏览器界面
    executablePath: '/path/to/chrome', // 指定浏览器可执行文件路径
    args: ['--no-sandbox', '--disable-setuid-sandbox'], // 额外参数
    defaultViewport: { width: 1280, height: 800 }, // 默认视口大小
    userDataDir: '/path/to/userDataDir'， // 用户数据目录
    slowMo: 100, // 延迟 100 毫秒
  });

  // 在这里可以进行其他操作，比如创建新页面、访问网页等

  await browser.close();
})();
````

在上面的代码中，我们通过 puppeteer.launch() 方法启动了一个浏览器实例，并通过 options 参数配置了一些选项，比如显示浏览器界面、指定浏览器可执行文件路径、传递额外参数、设置默认视口大小和用户数据目录。你可以根据需要自定义 options 对象中的属性来满足你的需求。

需要注意的是，在使用完浏览器实例后，应该调用 browser.close() 方法来关闭浏览器，释放资源。

## Browser 类

> Browser 类表示一个 Chrome 或 Chromium 浏览器实例。它提供了一组方法来操作整个浏览器，如创建新页面、关闭浏览器、监听事件等。

当 Puppeteer 连接到一个 Chromium 实例的时候会通过 puppeteer.launch 或 puppeteer.connect 创建一个 Browser 对象。

以下是一些 Browser 类常用的方法：

* **newPage():** 创建一个新的页面实例。

* **close():** 关闭浏览器实例。

* **version():** 获取浏览器的版本信息。

* **pages():** 获取所有已打开的页面实例。

* **newContext():** 创建一个新的浏览器上下文。

* **target():** 获取指定目标的实例。

下面是使用 Browser 创建 Page 的例子

```` javascript
const puppeteer = require('puppeteer');

puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await browser.close();
});
````

## Page 类

> Page 类表示一个浏览器页面。它提供了一系列方法，用于操作和控制页面的行为，例如导航至指定 URL、执行 JavaScript 代码、截取页面截图等。

以下是一些 Page 类常用的方法：

1. **goto(url):** 导航到指定的 URL
```` javascript
await page.goto('https://www.example.com');
````
   
2. **waitForSelector(selector):** 等待页面中指定的选择器出现。
```` javascript
await page.waitForSelector('.my-element');
````
   
3. **click(selector):** 点击页面中指定的选择器。
```` javascript
await page.click('.my-button');
````
   
4. **type(selector, text):** 在指定的输入框中输入文本。
```` javascript
await page.type('input[name="username"]', 'myusername');
````
   
5. **evaluate(pageFunction):** 在页面上下文中执行指定的函数。
```` javascript
const title = await page.evaluate(() => document.title);
````
   
6. **page.$$eval(selector, pageFunction, …args?):** 在页面中注入方法，执行 document.querySelectorAll 后将结果作为第一个参数传给函数体。
```` javascript
const texts = await page.$$eval('.my-elements', elements => elements.map(element => element.textContent));
````
   
## 页面操作

### 点击操作

* **page.click(selector, options?):** 点击选择器匹配的元素，有多个元素满足匹配条件仅作用第一个。
```` javascript
await page.click('.my-button');
````
  
* **page.tap(selector):** 点击选择器匹配的元素，有多个元素满足匹配条件仅作用第一个，主要针对手机端的触摸事件。
```` javascript
await page.tap('.my-button');
````
  
* **page.focus(selector):** 给选择器匹配的元素获取焦点，有多个元素满足匹配条件仅作用第一个。
```` javascript
await page.focus('.my-input');
````
  
* **page.hover(selector):** 鼠标悬浮于选择器匹配的元素，有多个元素满足匹配条件仅作用第一个。
```` javascript
await page.hover('.my-element');
````

### 输入操作

page.type 是 Puppeteer 中用于在指定元素上输入文本的方法。该方法接受两个参数：选择器和要输入的文本。

以下是 page.type 方法的用法示例：
```` javascript
await page.type('input[type="text"]', 'Hello, Puppeteer!');
````

在这个示例中，我们使用选择器 `input[type="text"]` 来定位页面上的一个文本输入框，并在该输入框中输入文本 'Hello, Puppeteer!'。

### 键盘模拟按键

page.keyboard 对象提供了一系列方法，可以模拟按键的按下、释放、输入等操作。

以下是一些常用的 page.keyboard 方法：

1. **keyboard.press(key[, options]):** 模拟按下指定的键。

2. **keyboard.release(key):** 模拟释放指定的键。

3. **keyboard.down(key):** 模拟按下指定的键，保持按下状态。

4. **keyboard.up(key):** 模拟释放指定的键，取消按下状态。

5. **keyboard.type(text[, options]):** 模拟输入指定的文本。

下面是一个示例代码，演示如何在输入框中模拟按键操作：
```` javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://www.example.com');

  // 获取要输入文本的输入框的选择器
  const selector = 'input[type="text"]';

  // 等待输入框加载完成
  await page.waitForSelector(selector);

  // 在输入框中模拟按键操作
  await page.focus(selector); // 让输入框获得焦点
  await page.keyboard.type('Hello, Puppeteer!'); // 输入文本
  await page.keyboard.press('Enter'); // 模拟按下 Enter 键

  await browser.close();
})();
````

在这个示例中，我们首先让输入框获得焦点，然后使用 page.keyboard.type 方法输入文本 'Hello, Puppeteer!'，最后使用 page.keyboard.press 方法模拟按下 Enter 键。

### 鼠标模拟

在 Puppeteer 中，可以使用 page.mouse 对象来模拟鼠标操作。page.mouse 对象提供了一系列方法，可以模拟鼠标的移动、点击、滚动等操作。

以下是一些常用的 page.mouse 方法：

1. **mouse.move(x, y[, options]):** 将鼠标移动到指定位置。

2. **mouse.click(x, y[, options]):** 在指定位置模拟鼠标点击。

3. **mouse.down([options]):** 模拟按下鼠标按钮。

4. **mouse.up([options]):** 模拟释放鼠标按钮。

5. **mouse.wheel(deltaX, deltaY):** 模拟滚动鼠标滚轮。

下面是一个示例代码，演示如何在页面中模拟鼠标操作：
```` javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://www.example.com');

  // 获取要点击的元素的选择器
  const selector = 'button';

  // 等待元素加载完成
  await page.waitForSelector(selector);

  // 获取元素的位置
  const element = await page.$(selector);
  const boundingBox = await element.boundingBox();

  // 在元素位置模拟鼠标点击
  await page.mouse.click(boundingBox.x + boundingBox.width / 2, boundingBox.y + boundingBox.height / 2);

  await browser.close();
})();
````

在这个示例中，我们首先等待页面中的按钮元素加载完成，然后获取按钮元素的位置信息，最后使用 page.mouse.click 方法在按钮元素的中心位置模拟鼠标点击操作。

## 事件监听

这些是 Puppeteer 中常用的页面事件，可以通过监听这些事件来执行相应的操作。以下是每个事件的简要说明：

* **close:** 当页面被关闭时触发。

* **console:** 当页面中调用 console API 时触发。

* **error:** 当页面发生错误时触发。

* **load:** 当页面加载完成时触发。

* **request:** 当页面收到请求时触发。

* **requestfailed:** 当页面的请求失败时触发。

* **requestfinished:** 当页面的请求成功时触发。

* **response:** 当页面收到响应时触发。

* **workercreated:** 当页面创建 webWorker 时触发。

* **workerdestroyed:** 当页面销毁 webWorker 时触发。

您可以通过以下示例代码来监听页面加载完成和页面请求成功的事件：

```` javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  // 监听页面加载完成事件
  page.on('load', () => {
    console.log('Page loaded successfully');
  });

  // 监听页面请求成功事件
  page.on('requestfinished', (request) => {
    console.log(`Request finished: ${request.url()}`);
  });

  await page.goto('https://www.example.com');

  await browser.close();
})();
````

在这个示例中，我们使用 page.on 方法来监听页面加载完成和页面请求成功的事件，并在事件发生时打印相应的信息。

## 等待元素、请求、响应

在 Puppeteer 中，您可以使用以下方法来等待元素、请求和响应：

1. **page.waitForXPath(xpath, options):** 等待指定的 XPath 对应的元素出现。参数 options 可以包含 timeout 和 visible 选项。返回一个 ElementHandle 实例。
```` javascript
await page.waitForXPath('//div[@class="example"]', { visible: true });
````

2. **page.waitForSelector(selector, options):** 等待指定的选择器对应的元素出现。参数 options 可以包含 timeout 和 visible 选项。返回一个 ElementHandle 实例。
```` javascript
await page.waitForSelector('.example', { visible: true });
````

3. **page.waitForResponse(predicate):** 等待符合条件的响应结束。参数 predicate 是一个函数，用于判断响应是否符合条件。返回一个 Response 实例。
```` javascript
const response = await page.waitForResponse(response => response.url().includes('/api'));
````

4. **page.waitForRequest(predicate):** 等待符合条件的请求出现。参数 predicate 是一个函数，用于判断请求是否符合条件。返回一个 Request 实例。
```` javascript
const request = await page.waitForRequest(request => request.url().includes('/api'));
````

这些方法可以帮助您在 Puppeteer 中更精确地控制等待元素、请求和响应的时间，以便在需要时执行相应的操作。如果您有任何疑问或需要进一步的解释，请随时告诉我。我将很乐意帮助您。

## 网络拦截操作

page.setRequestInterception() 方法可以拦截页面中发出的网络请求，并对其进行处理。通过拦截请求，你可以修改请求的行为，例如阻止请求、修改请求的头部、修改请求的内容等。

以下是使用 page.setRequestInterception() 方法的一个示例：

```` javascript
const puppeteer = require('puppeteer');
​
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
​
  // 启用请求拦截
  await page.setRequestInterception(true);
​
  // 监听请求事件
  page.on('request', (request) => {
    // 判断请求的 URL 是否符合条件
    if (request.url().includes('/api')) {
      request.continue(); // 继续请求
    } else {
      request.abort(); // 中止请求
    }
  });
​
  // 导航至指定 URL
  await page.goto('https://example.com');
  
    // 等待页面加载完成
  await page.waitForNavigation();
    
   // 获取符合条件的网络响应
  const responses = await page.waitForResponse(response => response.url().includes('/api'));
  // 获取接口数据
  const responseData = await responses.json();
  console.log(responseData);
  
  await browser.close();
})();
````

需要注意的是，在使用请求拦截功能时，务必要确保在请求被中止或继续之前，要么调用 interceptedRequest.abort() 中止请求，要么调用 interceptedRequest.continue() 继续请求，否则可能会导致页面无法正常加载。

## 简单示例

### 截图

> 在 Puppeteer 中实现截图可以通过 page.screenshot() 方法来实现。

以下是一个简单的示例代码，演示如何在 Puppeteer 中对页面进行截图：

```` javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    
    await page.goto('https://www.example.com');
    
    // 在当前目录下保存截图
    await page.screenshot({ path: 'example.png' });
    
    await browser.close();
})();
````

在上面的示例中，我们首先启动了一个 Puppeteer 浏览器实例，然后创建了一个新页面并访问了示例网站。接着使用 page.screenshot() 方法对页面进行截图，并将截图保存在当前目录下的 example.png 文件中。最后关闭了浏览器实例。

### 生成pdf

> 在 Puppeteer 中生成 PDF 可以通过 page.pdf() 方法来实现。

以下是一个简单的示例代码，演示如何在 Puppeteer 中生成 PDF：

```` javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    
    await page.goto('https://www.example.com');
    
    // 生成 PDF 并保存在当前目录下的 example.pdf 文件中
    await page.pdf({ path: 'example.pdf', format: 'A4' });
    
    await browser.close();
})();
````

在上面的示例中，我们首先启动了一个 Puppeteer 浏览器实例，然后创建了一个新页面并访问了示例网站。接着使用 page.pdf() 方法生成 PDF，并将其保存在当前目录下的 example.pdf 文件中。您还可以调整生成 PDF 的格式、尺寸、页面边距等参数。

### 设置cookie

> 在 Puppeteer 中设置 cookie 可以通过 page.setCookie() 方法来实现。

以下是一个简单的示例代码，演示如何在 Puppeteer 中设置 cookie：

```` javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    
    // 设置 cookie
    await page.setCookie({
        name: 'username',
        value: 'john_doe',
        domain: 'www.example.com'
    });
    
    await page.goto('https://www.example.com');
    
    // 在页面中获取 cookie
    const cookies = await page.cookies();
    console.log(cookies);
    
    await browser.close();
})();
````

在上面的示例中，我们首先启动了一个 Puppeteer 浏览器实例，然后创建了一个新页面。接着使用 page.setCookie() 方法设置了一个名为 username 的 cookie，然后访问了示例网站。最后使用 page.cookies() 方法获取页面中的所有 cookie，并将其打印出来。

您可以根据需要设置更多的 cookie，以及设置 cookie 的路径、过期时间等属性。