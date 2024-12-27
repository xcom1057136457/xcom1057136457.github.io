---
title: vue3.0中使用i18n插件来实现国际化以及切换语言
date: 2024-12-26 14:18:21
---

1. 下载il8n插件，目前通过`npm install vue-il8n`下载的il8n版本是无法支持vue3.0，因此要使用`npm install vue-i18n@next`来获取最新的版本，我这边是的版本是`^9.0.0-beta.17`
```js
npm install vue-i18n@next
yarn add vue-i18n@next
pnpm add vue-i18n@next(个人推荐)
```

2. 在src下面创建一个名为language的文件名，并在文件下面创建zh-CN、en、ja、ko四个js文件（中文、英文、日文、韩文）

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142111.png)

3. 然后在四个js文件中写入对应的方法，这里面存放的是我们要进行切换的语言，以下是我在名为ja.js文件写的日文语言，同样的方法在其他三个文件中依次写入

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142205.png)

4. 在language文件下面在创建一个index.js文件

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142228.png)

5. 在language文件下面在创建一个il8n的文件文件

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142256.png)

6. 在main.js中引入

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142316.png)

7. 在组件中使用`{{$t('message.storeBacode')}}`来显示在页面上

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142353.png)

8. 在js中使用，vue2.0中是`this.$t('message.storeBacode')`，但是在vue3.0中已经取消掉了this，因此为`proxy.$t('message.storeBacode')`

   1. 在vue中引入getCurrentInstance，在对getCurrentInstance进行挂载

      ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142535.png)

      ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142626.png)

    2. 在reactive中使用 **proxy.$t('message.unlimited')** 会报错
    
        ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226142937.png)

        ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143005.png)

   解决方法:引入 **useI18n()** ，并使用格式 `t('message.canceled')` 来进行页面渲染，同理在js中也可以这样使用，不需要proxy来操作，但是引入的时候，`const {t} = useIl8n()`要写在reactive前面

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143109.png)

   不需要proxy操作方法的编写

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143134.png)

    ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143148.png)

    **为什么用t来代替$t(下面的方法和$t并不冲突，之所以用t是为了解决在reactive中报错的问题)**

   $t是Vue18n实例的功能。它具有以下优点和缺点：可以在模板中灵活使用胡子语法{}，还可以在Vue组件实例中使用计算的道具和方法,但是缺点也很明显，每次发生重新渲染时都会执行，因此确实有翻译成本。

   v-t(t)具有更好的性能比$t功能，由于其前期的翻译是可能的这是由提供的Vue的编译器模块VUE,国际扩展性，可以进行更多性能优化。但是缺点同样也很明显：能像$t它那样灵活地使用，它相当复杂。与一起翻译的内容v-t会插入textContent元素的中。此外，当您使用服务器端渲染，您需要设置自定义转换到directiveTransforms了的选项compile功能@vue/compiler-ssr。

9. 组件内切换语言（重点）

    1. 误区（在vue2.0中是用this.$il8n.locale = 'cn' 这种方法来实现语言切换是正确，但是在vue3.0中虽然proxy相当于vue2.0中的this，但是这样切换语言是错误，后台会提示undefined,虽然此时的il8n确实挂载上去了，且locale也会报错，这个问题我也不知道是什么情况,改怎么去解决呢）

        ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143545.png)

        ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143602.png)

        ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143616.png)

    2. 解决方法（虽然不知道 是什么情况，但是最后问题还是解决了，我们来看解决思路）

        1. 打印proxy，然后找到il8n里面的availableLocales这个数组，看到我们前面要设置的几种语言，然后我们把它取出来

            ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143655.png)

        2. 取出il8n里面的availableLocales并把它渲染到select里面，当然在这之前要先做一下计算，不然打印出来的就是cn、en、ja、ka，作为开发者这个我们自己是能看懂得，但是对于用户用于，它是看不懂得

            **计算并转换（写了方法得小伙伴记得return出去，不然找不到方法会报错得哦）**

            ![](https://raw.githubusercontent.com/xcom1057136457/DrawingBed/main/20241226143751.png)