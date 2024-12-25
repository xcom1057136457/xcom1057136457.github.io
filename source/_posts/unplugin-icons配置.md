---
title: unplugin-icons配置
date: 2023-01-31 16:57:51
---
```` javascript
// 安装配置
yarn add unplugin-icons @iconify/json -D

// vite.config.ts
import Icons from 'unplugin-icons/vite'
import IconsResolver from 'unplugin-icons/resolver'
import Components from 'unplugin-vue-components/vite'

export default {
  plugins: [
    Components({
      resolvers: IconsResolver()
    }),
    Icons({
      compiler: 'vue3',
      autoInstall: true,
    })
  ]
}

// 使用示例
// 我们先打开网址：icones.netlify.app/ 随便选择一个图标
````

