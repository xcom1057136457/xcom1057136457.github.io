---
title: JSON动画渲染成图片
date: 2023-01-31 16:58:51
---
````
// 安装依赖
yarn add 'lottie-web'

// 使用
import lottie from 'lottie-web'
lottie.loadAnimation({
    container: <DOCUMENT节点>,
    renderer: 'svg',
    loop: true,
    autoplay: true,
    animationData: <JSON文件>
})
````

