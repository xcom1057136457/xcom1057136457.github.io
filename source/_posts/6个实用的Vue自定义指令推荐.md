---
title: 6个实用的Vue自定义指令推荐
lang: zh-CN
date: 2024-08-29 16:49:21
---
在 Vue2.0，除了核心功能默认内置的指令 ( v-model 和 v-show )，Vue 也允许注册自定义指令。在 Vue2.0 中，代码复用和抽象的主要形式是组件。然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。

Vue 自定义指令有全局注册和局部注册两种方式。全局注册指令的方式，通过 `Vue.directive( id, [definition] )` 方式注册全局指令。如果想注册局部指令，组件中也接受一个`directives`的选项。

``` javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

// 注册一个局部自定义指令 `v-focus`
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

然后我们可以在模板中任何元素上使用心得`v-focus` property，如下：

``` vue
<input v-focus>
```

当我们需要批量注册自定义指令时，写很多个``Vue.directive( id, [definition] )` 会导致代码冗余，所以我们可以利用`Vue.use()` 的特性，完成批量注册。

批量注册指令，新建 `directives/directive.js` 文件

``` javascript
// 导入指令定义文件
import debounce from './debounce'
import throttle from './throttle'
// 集成一起
const directives = {
  debounce,
  throttle,
}
//批量注册
export default {
  install(Vue) {
    Object.keys(directives).forEach((key) => {
      Vue.directive(key, directives[key])
    })
  },
}
```

在 `main.js` 引入，并`Vue.use()` 调用完成批量注册。

``` javascript
import Vue from 'vue'
import Directives from './directives/directive.js'
Vue.use(Directives)
```

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

- bind: 只调用一次，指令第一次绑定到元素时调用，可以定义一个在绑定时执行一次的初始化动作，此时获取父节点为null。
- inserted: 被绑定元素插入父节点时调用（仅保证父节点存在，但不一定已被插入文档中），此时可以获取到父节点。
- update: 所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
- componentUpdated: 指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- unbind: 只调用一次， 指令与元素解绑时调用。

接下来我们来看一下钩子函数的参数 (即 `el`、`binding`、`vnode` 和 `oldVnode`)。

指令钩子函数会被传入以下参数：

- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含以下 property：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

注意：除了 `el` 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 [`dataset`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/dataset) 来进行。

下面分享几个实用的 Vue 自定义指令

- 长按指令 `v-longpress`
- 函数防抖指令 `v-debounce`
- 函数节流指令 `v-throttle`
- 点击元素外部指令 `v-click-out`
- 弹窗限制外部滚动指令 `v-scroll-pop`
- 神策埋点指令`v-sensor`



## 1. v-longpress

需求：当用户按下鼠标左键或移动端单指触碰，并按住按钮几秒钟时，视为一次长按，触发对应的函数。

思路：

1. 定义一个计时器， n 秒后执行函数，n作为参数。
2. 当用户按下按钮时触发 `mousedown` 或`touchstart` 事件，启动计时器。
3. 如果 `click` 、 `mouseup` 、`touchend` 或 `touchcancel` 事件在 n 秒内被触发，则清除计时器，视为普通点击事件。
4. 如果计时器没有在 n秒内清除，则视为一次长按，触发对应的函数。

``` javascript
const longpress = {
  bind: function (el, {value:{fn,time}}) {
    //没绑定函数直接返回
    if (typeof fn !== 'function') return
    // 定义定时器变量
    el._timer = null
    // 创建计时器（ n秒后执行函数 ）
    el._start = (e) => {
      //e.type表示触发的事件类型如mousedown,touchstart等
      //pc端: e.button表示是哪个键按下0为鼠标左键，1为中键，2为右键
      //移动端: e.touches表示同时按下的键为个数
      if (  (e.type === 'mousedown' && e.button && e.button !== 0) || 
            (e.type === 'touchstart' && e.touches && e.touches.length > 1)
      ) return;
      //定时长按n秒后执行事件
      if (el._timer === null) {
        el._timer = setTimeout(() => {
          fn()
        }, time)
        //取消浏览器默认事件，如右键弹窗
        el.addEventListener('contextmenu', function(e) {
          e.preventDefault();
        })
      }
    }
    // 如果两秒内松手，则取消计时器
    el._cancel = (e) => {
      if (el._timer !== null) {
        clearTimeout(el._timer)
        el._timer = null
      }
    }
    // 添加计时监听
    el.addEventListener('mousedown', el._start)
    el.addEventListener('touchstart', el._start)
    // 添加取消监听
    el.addEventListener('click', el._cancel)
    el.addEventListener('mouseout', el._cancel)
    el.addEventListener('touchend', el._cancel)
    el.addEventListener('touchcancel', el._cancel)
  },
  // 指令与元素解绑时，移除事件绑定
  unbind(el) {
    // 移除计时监听
    el.removeEventListener('mousedown', el._start)
    el.removeEventListener('touchstart', el._start)
    // 移除取消监听
    el.removeEventListener('click', el._cancel)
    el.removeEventListener('mouseout', el._cancel)
    el.removeEventListener('touchend', el._cancel)
    el.removeEventListener('touchcancel', el._cancel)
  },
}

export default longpress
```

使用：给 Dom 加上 `v-longpress` 及参数即可

``` vue
<template>
  <button v-longpress="{fn: longpress,time:2000}">长按</button>
</template>

<script>
export default {
  methods: {
    longpress () {
      console.log('长按指令生效')
    }
  }
}
</script>
```



## 2. v-debounce

背景：在开发中，有时遇到要给input或者滚动条添加监听事件，需要做防抖处理。

需求：防止input或scroll事件在短时间内被多次触发，使用防抖函数限制一定时间后触发。

思路：

1. 定义一个延迟执行的方法，如果在延迟时间内再调用该方法，则重新计算执行时间。
2. 将事件绑定在传入的方法上。

``` javascript
const debounce = {
    inserted: function (el, {value:{fn, event, time}}) {
      //没绑定函数直接返回
      if (typeof fn !== 'function') return
      el._timer = null
      //监听点击事件，限定事件内如果再次点击则清空定时器并重新定时
      el.addEventListener(event, () => {
        if (el._timer !== null) {
          clearTimeout(el._timer)
          el._timer = null
        }
        el._timer = setTimeout(() => {
          fn()
        }, time)
      })
    },
  }
  
  export default debounce
```

使用：给 Dom 加上 `v-debounce` 及回调函数即可

``` vue
<template>
  <input v-debounce="{fn: debounce, event: 'input', time: 5000}" />
      <div v-debounce="{fn: debounce, event: 'scroll', time: 5000}">
          <p>文字文字文字文字...</p>
      </div>
</template>

<script>
export default {
  methods: {
    debounce(){
      console.log('debounce 防抖')
    },
  }
}
</script>
```



## 3. v-throttle

背景：在开发中，有些提交保存按钮有时候会在短时间内被点击多次，这样就会多次重复请求后端接口，造成数据的混乱，比如立即购买按钮，多次点击就会多次调用创建订单接口。

需求：防止按钮在短时间内被多次点击，使用节流函数限制规定时间内只能点击一次。

思路：

1. 定义一个由开关（默认为开）控制是否执行的方法，第一次执行函数时将开关关闭，在规定时间内再调用该方法，则不会再次执行，直至规定时间过后开关打开。
2. 将事件绑定在 click 方法上。

``` javascript
const throttle = {
    bind:function (el,{value:{fn,time}}) {
        if (typeof fn !== 'function') return
        el._flag = true;//开关默认为开
        el._timer = null
        el.handler = function () {
            if (!el._flag) return;
            //执行之后开关关闭
            el._flag && fn()
            el._flag = false
            if (el._timer !== null) {
                clearTimeout(el._timer)
                el._timer = null
            }
            el._timer = setTimeout(() => {
                el._flag = true;//三秒后开关开启
            }, time);
        }
        el.addEventListener('click',el.handler)
    },
    unbind:function (el,binding) {
        el.removeEventListener('click',el.handler)
    }
}

export default throttle
```

使用：给Dom加上`v-throttle` 及回调函数即可。

``` vue
<template>
 <button v-throttle="{fn: throttle,time:3000}">throttle节流</button>
</template>

<script>
export default {
  methods: {
    throttle () {
      console.log('throttle 节流 只触发一次')
    }
  }
}
</script>
```



## 4. v-clickOut

背景：在我们的项目里，经常会出现一个弹窗，需要点击弹窗外部关闭该弹窗。

需求：实现一个指令，点击目标区域外部，触发指定函数。

思路：

1. 判断点击的元素是否为目标元素，是则不作为，否则触发指定函数。

``` javascript
const clickOut = {
    bind(el,{value}){
        function clickHandler(e) {
            //先判断点击的元素是否是本身，如果是本身，则返回
            if (el.contains(e.target)) return;
            //判断指令中是否绑定了函数
            if (typeof value === 'function') {
                //如果绑定了函数，则调用函数，此处value就是clickImgOut方法
                value()
            }
        }
        // 给当前元素绑定个私有变量，方便在unbind中可以解除事件监听
        el.handler = clickHandler;
        //添加事件监听
        setTimeout(() => {
            document.addEventListener('click',el.handler);
        }, 0);
    },
    unbind(el){
        //解除事件监听
        document.removeEventListener('click',el.handler);
    }
}

export default clickOut
```

使用，将需要用到该指令的元素添加 `v-click-out`

``` vue
<template>
  <div>
 		<button @click="isImgShow = true">展示弹窗</button>
    <div v-click-out="clickImgOut" v-if="isImgShow" class="pop">
          <img src="https://xxx.jpg" alt="">
          <p>文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字文字</p>
    </div>
  </div>
</template>

<script>
  export default {
		data(){
      return {
        isImgShow : false
      }
    },
    methods:{
      clickImgOut(){
        this.isImgShow = false;
        console.log('点击弹窗外部')
      }
    }
	}
</script>
```



## 5. v-scrollPop

背景：在我们的项目中，经常使用弹窗展示活动规则，活动规则过长需要滚动时，时长会导致外部滚动。这时针对这种情况，我们可以通过全局自定义指令来处理。

需求：自定义一个指令，使得弹窗内部内容可以滚动，外部无法滚动。

思路：

1. 当弹窗展示时，记录滚动条滚动距离，然后给body和html设置固定定位，高度100%，top值为滚动距离。
2. 当弹窗解除时，恢复原先样式，并把滚动距离设置成原来的值。

``` javascript
const scrollPop = {
    bind(el) {
        //定义此时到元素的内容垂直滚动的距离
        el.st = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
        let cssStr = `overflow: hidden;width: 100%; height: 100%; position: fixed; top: ${- el.st}px;`
        document.querySelector('html').cssText = cssStr
        document.body.style.cssText = cssStr
    },
    unbind(el,{value}) {
        let cssStr = 'overflow: auto; height: 100%; position: relative; top: 0px;scroll-behavior: auto'
        document.querySelector('html').cssText = cssStr
        document.body.style.cssText = cssStr
        document.querySelector('html').style.scrollBehavior = 'auto'
        //手动设置滚动距离
        document.documentElement.scrollTop = el.st
        document.body.scrollTop = el.st
        if (value !== 'smooth')return;
        //如果传了滚动方式为smooth平稳滚动即有感滚动，当滚动完毕后，把auto改回smooth
        let timer = setTimeout(() => {
            cssStr = `overflow: auto; height: 100%; position: relative; top: 0px; scroll-behavior: ${value||'smooth'}`
            document.querySelector('html').cssText = cssStr
            document.querySelector('html').style.scrollBehavior = value || 'smooth'
            document.body.style.cssText = cssStr
        }, 1);
    }
}

export default scrollPop
```

使用：给需要限制的弹窗绑定`v-scroll-pop`属性,并设置`scroll-behavior` 值即可。

``` vue
<div class="scroll-pop" v-if="isScrollPopShow" v-scroll-pop="'smooth'">
	<div class="content">
    <p>这是很长一段文字，请耐心读完，然后你会发现这段文字并没有什么意义。</p>
    ...
  </div>
</div>
```



## 6. v-sensor

背景：目前前端埋点代码大量入侵业务，埋点代码量大且难以区分和维护，现做出优化方案以减少其代码量。

#### 埋点类型：

- **ElementShow：页面元素显示**
- **PopupTrack：弹窗显示**
- **$WebClick：点击页面按钮**
- **PopupBtnClick：点击弹窗中按钮**
- **自定义事件**

#### 优化方案：

##### 1.自定义指令 v-sensor=" {el :'Btn_XXX_Tag_Common',elClick:'Btn_XXX_Tag_Common'} "

 **注册封装自定义指令的代码**

``` javascript
const sensor = {
    // 当被绑定的元素插入到 DOM 中时
    inserted: function (el,{value: sensorObj}) {
        let showObj={} ,clickObj={}//showObj代表展示类埋点，clickObj代表点击类埋点
        //如果传入参数格式不为对象，则不向下执行
        if (!Object.prototype.toString.call(sensorObj) === '[object Object]'|| JSON.stringify(sensorObj) == "{}") return
        //遍历传入对象参数，根据key值确定埋点类型
        for (const key in sensorObj) {
            if (Object.hasOwnProperty.call(sensorObj, key)) {
                switch (key) {
                    case 'el':
                        showObj= {
                            name:'ElementShow',
                            value: sensorObj[key]
                        };
                        break;
                    case 'pop':
                        showObj= {
                            name:'PopupTrack',
                            value: sensorObj[key]
                        };
                        break;
                    case 'elClick':
                        clickObj= {
                            name:'$WebClick',
                            value: sensorObj[key]
                        };
                        break;
                    case 'popClick':
                        clickObj= {
                            name:'PopupBtnClick',
                            value: sensorObj[key]
                        };
                        break;  
                    default:
                        break;
                }
            }
        }
        // 展示类埋点执行
        showObj.value && sensors.track(showObj.name, {
            FileName: showObj.value
        });
        //点击类埋点执行
        if (clickObj.value) {
            el.handler = function () {
                clickObj.name === '$WebClick' && sensors.track(clickObj.name, {
                    $element_name: clickObj.value
                });
                clickObj.name === 'PopupBtnClick' && sensors.track(clickObj.name, {
                    FileName: clickObj.value
                });
            }
            el.addEventListener('click',el.handler)
        }
    },
    // 指令与元素解绑的时候，移除事件绑定
    unbind(el) {
        el.handler && el.removeEventListener('click', el.handler)
    }
}
  
  export default sensor
```

对于除自定义事件以外的埋点事件，较好的优化办法就是使用自定义指令。使用 v-sensor=" {el :'Btn_XXX_Tag_Common',elClick:'Btn_XXX_Tag_Common'} " 。v-sensor接收一个对象作为参数，对象的key为事件标识，对象的value为事件属性，key值具体对应关系如下。

- el：ElementShow
- pop：PopupTrack
- elClick：$WebClick
- popClick：PopupBtnClick

``` vue
//单独使用ElementShow或$WebClick
<div v-sensor="{el :'Btn_XXX_Tag_CXXXon'}">我是一个么得感情的标签</div>
<div v-sensor="{elClick:'Btn_XXX_Tag_Common'}">俺也一样</div>
//ElementShow和$WebClick组合使用方法
<div v-sensor="{el :'Btn_XXX_Tag_Common',elClick:'Btn_XXX_Tag_Common'}">俺也一样</div>
//单独使用PopupTrack和PopupBtnClick
<div v-sensor="{pop :'Pop_XXX_Tag_Common'}">俺也一样</div>
<div v-sensor="{popClick:'Pop_XXX_Tag_Common'}">俺也一样</div>
//PopupTrack和PopupBtnClick组合使用方法
<div v-sensor="{pop :'Pop_XXX_Tag_Common',popClick:'Pop_XXX_Tag_Common'}">俺也一样</div>
//变量使用方法
<div v-sensor="{pop :`${sensorVal}`}">俺也一样</div>
```

提示：

 由于该自定义指令是在元素插入页面DOM中时执行的，所以如果事件属性值使用变量的话，请在created生命周期内操作完毕，或给该元素绑定v-if为对应变量。
