---
title: vite权限路由
date: 2023-01-31 16:57:25
---
```` javascript
import { constantRoutes } from '@/router'
import { getRouters } from '@/api/menu'
import Layout from '@/layout/index.vue'
import ParentView from '@/components/ParentView/index.vue'
import { defineStore } from 'pinia'

// 匹配views里面所有的.vue文件
const modules = import.meta.glob('../views/**/*.vue')

/**
 * TODO: 之后把any替换为对应的interface
 * */

export const usePermissionStore = defineStore({
  id: 'permission',
  state: () => {
    return {
      routes: [],
      addRoutes: [],
      defaultRoutes: [],
      topbarRouters: [],
      sidebarRouters: []
    }
  },
  actions: {
    setRoutes(routes: any) {
      this.addRoutes = routes
      this.routes = constantRoutes.concat(routes) as any
    },
    setDefaultRoutes(routes: any) {
      this.defaultRoutes = constantRoutes.concat(routes) as any
    },
    setTopbarRoutes(routes: any) {
      this.topbarRouters = routes
    },
    setSidebarRouters(routes: any) {
      this.sidebarRouters = routes
    },
    generateRoutes() {
      return new Promise((resolve) => {
        // 向后端请求路由数据
        getRouters().then((res) => {
          const sdata = JSON.parse(JSON.stringify(res.data))
          const rdata = JSON.parse(JSON.stringify(res.data))
          const defaultData = JSON.parse(JSON.stringify(res.data))
          const sidebarRoutes = filterAsyncRouter(sdata)
          const rewriteRoutes = filterAsyncRouter(rdata, false, true)
          const defaultRoutes = filterAsyncRouter(defaultData)
          this.setRoutes(rewriteRoutes)
          this.setSidebarRouters(constantRoutes.concat(sidebarRoutes))
          this.setDefaultRoutes(sidebarRoutes)
          this.setTopbarRoutes(defaultRoutes)
          resolve(rewriteRoutes)
        })
      })
    }
  }
})

// 遍历后台传来的路由字符串，转换为组件对象
function filterAsyncRouter(
  asyncRouterMap: any,
  lastRouter = false,
  type = false
) {
  return asyncRouterMap.filter((route: any) => {
    if (type && route.children) {
      route.children = filterChildren(route.children)
    }
    if (route.component) {
      // Layout ParentView 组件特殊处理
      if (route.component === 'Layout') {
        route.component = Layout
      } else if (route.component === 'ParentView') {
        route.component = ParentView
      } else {
        route.component = loadView(route.component)
      }
    }
    if (route.children != null && route.children && route.children.length) {
      route.children = filterAsyncRouter(route.children, route, type)
    } else {
      delete route.children
      delete route.redirect
    }
    return true
  })
}

function filterChildren(childrenMap: any, lastRouter: any = false) {
  let children: any = []
  childrenMap.forEach((el: any, index: number) => {
    if (el.children && el.children.length) {
      if (el.component === 'ParentView' && !lastRouter) {
        el.children.forEach((c: any) => {
          c.path = el.path + '/' + c.path
          if (c.children && c.children.length) {
            children = children.concat(filterChildren(c.children, c))
            return
          }
          children.push(c)
        })
        return
      }
    }
    if (lastRouter) {
      el.path = lastRouter.path + '/' + el.path
    }
    children = children.concat(el)
  })
  return children
}

export const loadView = (view: any) => {
  let res
  for (const path in modules) {
    const dir = path.split('views/')[1].split('.vue')[0]
    if (dir === view) {
      res = () => modules[path]()
    }
  }
  return res
}
````

