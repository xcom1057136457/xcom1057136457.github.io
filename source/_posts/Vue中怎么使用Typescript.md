---
title: Vue中怎么使用Typescript
date: 2021-02-22 18:06:27
---
> 之前一直听说Vue+Typescript体验有多好，这次来记录一下步骤以及过程

这边是结合`vue-property-decorator`使用ts。

使用ts的vue项目，和js项目结构是类似的，多了几个文件

* tsconfig.json: typescript配置文件，主要用于指定待编译的文件和定义编译选项
* shims-tsx.d.ts: 允许.tsx 结尾的文件，可在 Vue 项目中编写 jsx 代码
* shims-vue.d.ts: 主要用于 TypeScript 识别.vue 文件，Ts 默认并不支持导入 vue 文件

### 配合ts使用的库

- vue-class-component: 类的装饰器
- vue-property-decorator: 基于 vue 组织里 vue-class-component 所做的拓展`import { Vue, Component, Inject, Provide, Prop, Model, Watch, Emit, Mixins } from 'vue-property-decorator'`
- vuex-module-decorators: 用 typescript 写 vuex 很好用的一个库`import { Module, VuexModule, Mutation, Action, MutationAction, getModule } from 'vuex-module-decorators'`

### 组件声明

组件声明乍一看有点复杂，以前就是`export default {name:'Home',data:...}`

现在必须拆分成以下这样：

``` javascript
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

@Component({
  name:'Home'
  components: {
    HelloWorld
  }
})
export default class Home extends Vue {}
```

### data对象

data在ts里是直接展开属性的：

``` javascript
// 可以在上面使用{{name}}
export default class Home extends Vue {
  private name = "哈哈哈";
}
```

### props声明

使用`@props`

``` javascript
export default class HelloWorld extends Vue {
  // <hello-world msg="颜酱">
  @Prop({ default: "" }) private msg!: string;
}
```

* !: 表示一定存在，?: 表示可能不存在。这两种在语法上叫赋值断言
* @Prop(options: (PropOptions | Constructor[] | Constructor) = {})
  - PropOptions，可以使用以下选项：type，default，required，validator
  - Constructor[]，指定 prop 的可选类型
  - Constructor，例如 String，Number，Boolean 等，指定 prop 的类型

### methods

直接展开写就行

``` javascript
export default class HelloWorld extends Vue {
  // methods <div @click="clickName">
  public clickName(): void {
    console.log(this.name);
    console.log(this.msg);
  }
}
```

### watch

watch也是分两块：

``` javascript
export default class HelloWorld extends Vue {
  // watch，函数和参数分为两部分，函数名必须是on[key]Change
  @Watch("name", { immediate: true, deep: true })
  private onNameChange(newValue: string, oldValue: string) {
    console.log("watch", newValue, oldValue);
  }
}
```

* @Watch(path: string, options: WatchOptions = {})
* options 包含两个属性 immediate?:boolean 侦听开始之后是否立即调用该回调函数 / deep?:boolean 被侦听的对象的属性被改变时，是否调用该回调函数
* @Watch('arr', { immediate: true, deep: true }) onArrChanged(newValue: number[], oldValue: number[]) {}

### computed

computed直接展开，就是普通方法加个`get`

``` javascript
export default class HelloWorld extends Vue {
  // computed 就是普通方法前面加个get，表示此属性是计算来的 <div>{{allname}}</div>
  public get allname() {
    return "computed " + this.name;
  }
}
```

### 生命周期函数

生命周期函数就是普通函数的感觉

``` javascript
export default class HelloWorld extends Vue {
  // created生命周期函数
  public created(): void {
    console.log("created");
  }
  // mounted生命周期函数
  public mounted(): void {
    console.log("mounted");
  }
}
```

### emit

emit稍微绕了点：

``` javascript
export default class HelloWorld extends Vue {
  
  /* 以下相当于js时候的 
  clickMsg(){
    this.name='xx';
    this.$emit('change-msg','ddd')
  } 
  当前组件 <div @click="clickMsg">
  父组件 <hello-world :msg="msg" @change-msg="handleChangeMsg">
  handleChangeMsg(value){
    this.msg = value
  }
  
  */
  @Emit("change-msg")
  clickMsg() {
    // 组件本身的逻辑
    this.name = "点击msg 之后 可以修改名字";
    // emit的第二个参数，也就是给父组件的值
    return "ddd";
  }
}
```

* @Emit(event?: string)
* @Emit 装饰器接收一个可选参数，该参数是emit的第一个参数，充当事件名，没有提供这个参数的话，Emit 会将回调函数名的 camelCase 转为 kebab-case，并将其作为事件名
* @Emit 会将回调函数的返回值作为第二个参数，如果返回值是一个 Promise 对象，$emit 会在 Promise 对象被标记为 resolved 之后触发
* @Emit 的回调函数的参数，会放在其返回值之后，一起被$emit 当做参数使用
