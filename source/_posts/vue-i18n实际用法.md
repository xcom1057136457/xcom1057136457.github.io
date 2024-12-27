---
title: vue-i18n实际用法
date: 2024-12-27 10:30:24
---

1. 安装: `pnpm add vue-i18n@next`

2. 创建language文件夹

3. 在language下创建index.ts

````javascript
import { createI18n } from 'vue-i18n'

const modules: any = import.meta.glob('./modules/*.ts', { eager: true })

const messages: any = {}

Object.keys(modules).forEach((lang: any) => {
  const name = lang.split('/').pop().split('.')[0]
  messages[name] = modules[lang].default
})

const i18n = createI18n({
  locale: localStorage.getItem('guide-language') || 'cn',
  fallbackLocale: 'cn',
  messages,
  legacy: false,
  globalInjection: true,
})

export default i18n
````
   
4. 在language文件夹下创建modules，并放入对应的语言ts文件，如

````javascript
export default {
  test: '测试',
}
````

5. 使用

````javascript
import i18n from '@/language/index.ts'

app.use(i18n)
````
   
6. 创建store文件，language.ts

````javascript
export const useLanguageStore = defineStore('language', () => {
  const { locale } = useI18n()

  const language = useLocalStorage('guide-language', 'cn')

  const changeLanguage = (lang: string) => {
    language.value = lang
    locale.value = lang
  }

  return {
    language,
    changeLanguage,
  }
})
````