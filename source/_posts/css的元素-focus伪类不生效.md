---
title: "css的元素:focus伪类不生效"
date: 2023-05-05 10:11:56
tags:
---

#### 原因：

div 是不支持:focus 伪类 的，因为 div 等元素无法接受键盘或其他用户事件

#### 方法：

div 元素添加 tabIndex 属性，使其能够支持:focus 伪类
`<div tabindex="1"></div>`
