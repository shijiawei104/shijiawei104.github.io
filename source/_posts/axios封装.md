---
layout: axios
title: axios 封装
date: 2022-11-23 14:54:32
tags:
---
# axios封装

```
  import axios from "axios";
    
    // 使用create创建axios实例
    const Service=axios.create({
    timeout:8000,
    baseURL:"http:xxxxx/",
    headers:{
        "Content-type":"application/json;charset=utf-8",
        "Authorization" : 登陆后拿到的token
    }
    })

    // 请求拦截-增加loading,对请求做统一处理
    Service.interceptors.request.use(config=>{
    loadingObj=ElLoading.service({
        lock: true,
        text: 'Loading',
        background: 'rgba(0, 0, 0, 0.7)',
      })
      return config
    })

    // 响应拦截-对返回值做统一处理
    Service.interceptors.response.use(response=>{
    const data=response.data
    return data
    },error=>{
    })

    // post请求
    export const post=config=>{
    return Service({
    ...config,
    method:"post",
    data:config.data
    })
    }
    
    // get请求
    export const get=config=>{
    return Service({
    ...config,
    method:"get",
    params:config.data
    })
    }
```
##### 接口封装
    import {get,post} from "./service"
    export const loginApi=data=>{
    return post({
        url:'/login',
        data
    })
    }

##### 使用
    import {loginApi} from "@/util/request"
    
    loginApi(data).then(res=>{})

