---
title: Vue修改数据页面不更新的问题解决
date: 2022-12-09 14:12:25
categories: 
- vue
tags:
---
### 1.:Vue.$set(官方推荐)
this.$set(this.item, "newProperty", "新值");
### 2.Vue.$forceUpdate(手动强制更新视图)
this.item.newProperty = "新值" 
this.$forceUpdate();
### 3.Object.assign(使用修改栈能触发视图更新的特性,借鉴React的写法)
this.item = Object.assign({},this.item,{newProperty:'新值'})
### 4.elementui 重置表单
this.$refs.form.resetFields();