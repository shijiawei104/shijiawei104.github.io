---
title: vue3+ts报错处理
date: 2023-05-09 09:41:27
tags:
---

#### 类型“never”上不存在属性“xxx”

#### 类型“any”的参数不能赋给类型“never”的参数

解决办法：

````js
const data = reactive({
  list: [] as any[] // 属性需要做类型断言处理
});```
````
