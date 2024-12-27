---
title: Vite使用mockjs
date: 2024-12-27 11:04:44
---

## 1. 安装mockjs

````javascript
pnpm add mockjs@1.0.0

pnpm add @types/mockjs -D
````

## 2. 安装vite-plugin-mock

````javascript
pnpm add vite-plugin-mock@2.9.6 -D
````

## 3. vite.config.ts

````javascript
import { viteMockServe } from 'vite-plugin-mock'

viteMockServe({
  localEnabled: true,
  prodEnabled: true,
  mockPath: 'mock',
  injectCode: `
    import { setupProdMockServer } from './plugins/mockProdServer'
    setupProdMockServer()
  `,
}),
````

## 4. 在plugins中加入 mockProdServer.ts

````javascript
import { createProdMockServer } from 'vite-plugin-mock/es/createProdMockServer'

const modules = import.meta.glob('../../mock/*.mock.ts', { eager: true })

const apiList: any[] = []

Object.values(modules).forEach((module: any) => {
  apiList.push(...module.default)
})

export function setupProdMockServer() {
  createProdMockServer([...apiList])
}
````
