---
title: vite-plugin-svg-icons配置
date: 2024-12-27 11:07:30
---

````javascript
// vite.config.ts
import { createSvgIconsPlugin } from 'vite-plugin-svg-icons'

export default defineConfig({
    plugins: [
        createSvgIconsPlugin({
            // 指定需要缓存的图标文件夹
            iconDirs: [resolve(process.cwd(), 'src/assets/icons')],
            // 指定symbolId格式
            symbolId: 'icon-[name]',
            inject: 'body-last',
            customDomId: '__svg__icons__dom__'
        })
    ]
})

// main.ts
import 'virtual:svg-icons-register'

// SvgIcon.vue
<script lang="ts" setup>
import { computed } from 'vue'

defineOptions({
  name: 'SvgIcon',
})

const props = withDefaults(defineProps<IProps>(), {
  className: '',
  color: '',
})

interface IProps {
  iconClass: any
  className?: string
  color?: string
}

const iconName = computed(() => {
  return `#icon-${props.iconClass}`
})

const svgClass = computed(() => {
  if (props.className)
    return `svg-icon ${props.className}`

  return 'svg-icon'
})
</script>

<template>
  <svg :class="svgClass" aria-hidden="true">
    <use :xlink:href="iconName" :fill="color" />
  </svg>
</template>

<style lang="scss" scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  position: relative;
  fill: currentColor;
  vertical-align: -2px;
}
</style>
````