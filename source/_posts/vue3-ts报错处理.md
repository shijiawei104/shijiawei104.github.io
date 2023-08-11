---
title: vue3+ts报错处理
date: 2023-05-09 09:41:27
tags:
---

#### 1.类型“never”上不存在属性“xxx”

#### 类型“any”的参数不能赋给类型“never”的参数

解决办法：

````js
const data = reactive({
  list: [] as any[] // 属性需要做类型断言处理
});```
````

#### 2.引入第三方模块报错

Could not find a declaration file for module ‘xxx‘. vue + ts
解决办法：

在 shims-vue.d.ts 文件中加入代码 declare module '引入的模块名称'

或者在安装时 npm i --save @types/插件名称
