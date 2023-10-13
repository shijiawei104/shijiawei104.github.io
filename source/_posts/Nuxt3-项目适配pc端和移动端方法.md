---
title: Nuxt3 项目适配pc端和移动端方法
date: 2023-10-13 14:02:55
tags:
---

#### 一.如果是 pc 端和移动端的页面样式相差不大的情况下可以使用 css 媒体查询

#### 二.如果是页面相差较大的情况下推荐写两套代码，判断设备类型，跳转到不同的页面

1.Nuxt 提供了一个可定制的路由中间件框架，可以根据这个中间件判断设备类型进行不同的跳转
路由中间件分为三种：
匿名（或内联）路由中间件，在使用它们的页面中直接定义。
具名路由中间件，放置在 middleware/目录下，在页面使用时会异步导入自动加载。（注意：路由中间件名称被标准化为 kebab-case 也叫连字符命名法，所以 someMiddleware 变成了 some-middleware。）
全局路由中间件，放置在 middleware/目录下（后缀为.global），每次路由变化时自动运行。
前两种路由中间件可以在 definePageMeta 中定义。
具体的可以参考官方文档

```js
export default defineNuxtRouteMiddleware((to, from) => {
  if (process.server) {
    // 在服务器端处理路由
    const nuxtApp = useNuxtApp();
  } else {
    // 在客户端处理路由

    // 是否是移动端设备
    const isMobile =
      /(Android|webOS|iPhone|iPod|tablet|BlackBerry|Mobile)/i.test(
        navigator.userAgent
      );

    // 是否是手机端路由
    const isRouterMobile = /^\/m\//.test(to.fullPath);

    // 移动端并且 不是/m开头路由
    if (isMobile && !isRouterMobile) {
      return navigateTo(`/m${to.fullPath}`);
    }

    // 不是移动端 是/m开头路由
    if (!isMobile && isRouterMobile) {
      return navigateTo(`${to.fullPath.replace("/m", "")}`);
    }
  }
});
```

2.上面的利用中间件跳转的方法我在实际操作过程中发现项目还是会先加载默认的 index 路由，会有一瞬间的卡顿，所以选择利用@nuxtjs/device 插件判断设备类型，加载不同的组件

```js
 npm install -D @nuxtjs/device
```

在 nuxt.config 配置

```js
modules: ["@nuxtjs/device"];
```

使用：
由于 nuxt3 的特性，直接在 dom 中使用$device.isDesktop 判断时，在服务端会判断失效，因此需要以下方法

```js
<script setup>
    const { isDesktop } = useDevice();
    const state = ref(null)
    onMounted(async () => {
	await nextTick()
	state.value = isDesktop
});
</script>
```
