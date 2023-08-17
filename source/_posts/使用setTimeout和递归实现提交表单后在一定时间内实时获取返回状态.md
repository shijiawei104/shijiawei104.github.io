---
title: 使用setTimeout和递归实现提交表单后在一定时间内实时获取返回状态
date: 2023-08-17 10:52:35
tags:
---

```js
// 应用场景：提交表单后获取提交完成状态，如果是自己想要的状态则跳转到指定页，该方法最多执行5次为15秒，5次后不管是不是等于想要的状态都会进行跳转
const time = ref();
const stateNum = ref(1);
const getState = () => {
  if (stateNum.value < 6) {
    time.value = setTimeout(() => {
      queryApplyApi().then((res: any) => {
        if (res.state === "NEED_SIGN" || res.state === "REJECTED") {
          clearTimeout(time.value);
          router.push({ path: "/merchant-state" });
        } else {
          stateNum.value++;
          getState();
        }
      });
    }, 3000);
  } else {
    clearTimeout(time.value);
    router.push({ path: "/merchant-state" });
  }
};
```
