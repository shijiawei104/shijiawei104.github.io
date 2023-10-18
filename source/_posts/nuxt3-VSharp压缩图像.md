---
title: nuxt3+VSharp压缩图像
date: 2023-10-18 16:15:41
tags:
---

安装：npm i vite-plugin-vsharp -D
nuxt.congig 中配置

```js
import vsharp from "vite-plugin-vsharp";

export default defineNuxtConfig({
  // 👇 添加 vsharp 到 vite 插件
  vite: {
    plugins: [vsharp()],
  },
});
```

然后打包时就会自动对图片进行压缩
