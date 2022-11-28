---
title: uniapp request封装
date: 2022-11-28 15:14:11
categories:
- vue
tags:
---
```js
const config = require('@/config/index.js'); // 请求的base URL
 
const getTokenHandle = () => {  
    return uni.getStorageSync('token')  // 获取登录用户token
}

// // 判断是否有token,没有跳转到登录页
//    if (!getTokenHandle) {
//        uni.reLaunch({
//          url: "/pages/signIn/signIn",
//        });
//    } else {
//      uni.reLaunch({
//        url: "/pages/power-station/index",
//      });
//    }
// 请求参数处理
const requestParam = (data, method) => {
    let str = '';
    if (method == 'post') {
        str = JSON.stringify(data);
        return str;
    } else if (method == 'get') {
        for (let i in data) {
            str += i + '=' + data[i] + '&';
        }
        str = str.length > 0 ? str.substring(0, str.length - 1) : '';
        return encodeURI(str);
    }
};
 
 
// 请求封装
const sendRequest = (method = 'GET', url = '', params = {}, ) => {
    let promise = new Promise(async (resolve, reject) => {
        let token = await getTokenHandle();
		console.log(getTokenHandle())
        const URL = config.default.url + '/wxmp'+ url + (method === 'GET' ? params : '')
        //网络请求
        uni.request({
            header: {
                Authorization: 'Bearer ' + token
            },
            url: URL,
            method,
            data: method === 'GET' ? {} : params,
            success: function(res) {
                // 响应403拦截跳转登录
                // if (res.statusCode == 200 && res.data.status === 403) {
                //     resInterceptHandle()
                //     return
                // }
                // 服务器返回数据
                if (res.statusCode == 200) {
                    // resolve(res.data);
					if(res.data.code==401&&res.data.msg=="账号未登录"){
						uni.showToast({
							icon: 'none',
							title: res.data.msg,
							duration: 2000,
							mask: true
						});
						uni.reLaunch({
						  url: "/pages/signIn/signIn",
						});
					}else if(res.data.code!=0){
						uni.showToast({
							icon: 'none',
							title: res.data.msg,
							duration: 2000,
							mask: true
						});
					}else{
						resolve(res.data);
					}
                } else {
                    console.log(`============${method} 请求失败：============`);
                    console.error(
                        '接口地址: ',
                        '【 ' + URL + ' 】'
                    );
                    console.error('请求参数: ', params);
                    console.error('返回结果: ', res);
                    uni.showToast({
                        title: res.data.emsg,
                        icon: 'none',
                        duration: 3000,
                        mask: true
                    });
                    reject(res);
                }
            },
            fail: function(res) {
				uni.showToast({
				    title: '请求失败',
				    icon: 'none',
				    duration: 3000,
				    mask: true
				});
                console.log(`============${method} 请求失败：============`);
                console.error(
                    '接口地址: ',
                    '【 ' + URL + ' 】'
                );
                console.error('请求参数: ', params);
                console.error('返回结果: ', res);
                reject(res);
            }
        });
    });
    return promise;
}
 
 
// 封装post请求（url:请求接口； data：请求参数；）
const httpPost = async (url, data) => {
    const params = requestParam(data, 'post');
    const res = await sendRequest('POST', url, params)
    return res
};
 
// 封装get请求（url:请求接口； data：请求参数；）
const httpGet = async (url, data) => {
    let params = data && JSON.stringify(data) != '{}' ?
        '?' + requestParam(data, 'get') :
        '';
    const res = await sendRequest('GET', url, params)
    return res
};
 
// 自定义请求类型
const http = async (params = {
    method: 'GET',
    url: '',
    data: {}
}) => {
    const res = await sendRequest(params.method, params.url, params.data)
    console.log(`自定义${params.method}请求成功响应`, res)
    return res
};
 
module.exports = {
    Post: httpPost,
    Get: httpGet,
    http: http
};
```
```js

const {Get,http,Post} = require("@/request/index");

export default{
	// 登录
    login(data) {
        return Post('/auth/login', data);
    },
	// 天气
	weather(data){
		return Get('/weather/info',data);
	},
}
```
```js
    import api from '@/request/api.js'
    api.login(data).then(res=>{})
```


