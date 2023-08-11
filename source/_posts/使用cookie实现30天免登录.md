---
title: 使用cookie实现30天免登录
date: 2023-08-11 10:15:49
tags:
---

安装：npm install js-cookie --save

```js
import Cookies from "js-cookie";

// 存放
// 名称/参数/有效期
export const setToken = (token: string) => {
  Cookies.set(CacheKey.TOKEN, token, { expires: 30 });
};
set方法支持的属性
expires
定义有效期。如果传入Number，那么单位为天，你也可以传入一个Date对象，表示有效期至Date指定时间。默认情况下cookie有效期截止至用户退出浏览器。
path
string，表示此cookie对哪个地址可见。默认为”/”。
domain
string，表示此cookie对哪个域名可见。设置后cookie会对所有子域名可见。默认为对创建此cookie的域名和子域名可见。
secure
true或false，表示cookie传输是否仅支持https。默认为不要求协议必须为https。

// 取出
export const getToken = () => {
  return Cookies.get(CacheKey.TOKEN);
};


```
