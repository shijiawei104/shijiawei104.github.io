---
title: 识别身份证日期在ios端显示 invalid Date
date: 2023-08-17 10:28:52
tags:
---

#### 在进行身份证国徽面识别时，阿里云的识别接口返回的日期格式为"xxxx.xx.xx-xxxx.xx.xx",通常我们需要再对该数据进行截取开始日期和结束日期进行显示选择，在使用 dayjs("xxxx.xx.xx")或者 new Date("xxxx.xx.xx")根据需求转换时在 ios 端会报错 “invalid Date”，这是因为 ios 端不支持这样的传参

解决方法：

```js
const formatdate = (dateStr: string) => {
  return dateStr.replace(/\./g, "/"); // 将 "." 替换为 "/"
};
```
