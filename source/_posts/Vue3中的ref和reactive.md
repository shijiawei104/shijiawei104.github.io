---
title: Vue3中的ref和reactive
date: 2023-05-05 14:12:59
tags:
---

ref 和 reactive 是 Vue3 中用来实现数据响应式的 API
一般情况下，ref 定义基本数据类型，reactive 定义引用数据类型

```js
// 一： ref 定义(ref 定义的数据，赋值时要加上.value 实现响应式)
const tableData = ref([]);
const getTableData = async () => {
  const { data } = await getTableDataApi();
  tableData.value = data; // 使用.value重新赋值
};
// 二：reactive定义，使用 push 方法
const tableData = reactive([]);
const getTableData = async () => {
  const { data } = await getTableDataApi();
  tableData.push(...data); // 先使用...将data解构，再使用push方法
};
// 三：reactive定义时数组外层嵌套一个对象
const tableData = reactive({ list: [] });
const getTableData = async () => {
  const { data } = await getTableDataApi();
  tableData.list = data; // 通过访问list属性重新赋值
};
// 四：赋值前再包一层 reactive
const tableData = reactive([]);
const getTableData = async () => {
  const { data } = await getTableDataApi();
  tableData = reactive(data); // 赋值前再包一层reactive
};
```
