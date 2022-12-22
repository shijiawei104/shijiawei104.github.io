---
title: es6随笔
date: 2022-12-14 10:48:05
categores:
  - js
tags:
---

```js
// es5
function ff(a,b){
    return a+b;
}
// es6
// 多个参数
let ff1 = (a,b) =>{
    return a+b;
}

// 一个参数
let ff2 = a =>{
    return a;
}
// 返回值
let ff3 = a => a;

// 返回数组
let ff4 = a => [];

// 返回对象
let ff5 = a => ({});

数组方法
let arr = [1,2,3]
// 获取数组最后一个人元素
arr[arr.length-1];   //3

// 数组转换为字符串
arr.toString();     //1,2,3

// 以对应的符号分割为字符串
arr.join("/")      //1/2/3

// 在数组开头添加元素
arr.unshift("a")    //a,1,2,3

// 在数组尾部添加元素
arr.push("a")       //1,2,3,a

// 在数组中删除最后一个元素(pop()返回的是弹出的值)
arr.pop()           //[1,2]

// 在数组中删除第一个元素(shift()返回的是弹出的值)
arr.shift()         //[2,3]

// 修改元素
// splice方法可删除对应的元素
arr.splice(位置，个数，添加的元素)
arr.splice(1,1)  //[1,3]
// 添加或替换对应的元素
arr.splice(1,0,a)  //[1,a,2,3]
arr.splice(1,2,a)  //[1,a]

// slice返回一个新数组，截取对应的字段
arr.slice(1,3)   //[2,3]

// concat()合并数组,不改变原有数组
let arr2 = [a,b]
arr.concat(arr2)  //[1,2,3,a,b]

// sort 排序
let a = [2,3,1,11]
a.sort()              //1,11,2,3
a.sort((a,b) => a-b)  //1,2,3,11

// reverse 用于颠倒数组中元素的顺序,会改变原数组
arr.reverse()        //[3,2,1]

// 遍历数组
// for循环遍历
for(let i=0; i<arr.length; i++){}
// forEach()遍历
arr.forEach((item, index) => {})

// 筛选数组
// filter 返回满足条件的数组,不改变原数组
let newarr = arr.filter((item,index)=>{
    return item > 1;         //[2,3]
})
// every  返回每一项都满足条件的数组为true或false
let newarr = arr.every((item,index)=>{
    return item > 1;         //false
    // 每一个数字都大于1才返回true
})
// some 只要有一个满足就返回true
let newarr = arr.some((item,index)=>{
    return item > 1;         //true
})

对象方法
// 新建对象
new object()
{}

//属性的获取方式
.    //取自身的属性
[]   //可以是变量

// 枚举属性
// for in 遍历对象的属性
let obj = {name:'a', age:18, id:1}
for(item in obj){
    item        //name age id  获取键名
    onj[item]   //a 18 1  获取对应value值
}

// 序列化  数据json字符串的方式来传输
// 对象转json字符串
JSON.stringify()
// json字符串转对象
JSON.parse()

// hasOwnProperty 判断对象属性书否存在
obj.hasOwnProperty('name')   //true

// assign 对象的合并(有相同属性时后面的会覆盖前面的属性值，属性不相同添加属性)
let obja = {a:1}
let objb = {b:2}
Object.assign(obja, objb)  //{a:1,b:2}

// defineProperty 直接在一个对象上定义新的属性或者修改现有的属性，并返回该对象
let objc = {};
Object.defineProperty(objc, 'name', {
    value: '张三',
    // 是否可以基于delete删除
    configurable: false,
    // 是否是可以枚举的，如果是不可枚举的属性，则基于for/in循环或者Object.keys等操作都无法获取这个属性
    enumerable: false,
    // 是否可以修改值「不能和get/set同时设置」
    // writable: true,
    get() { },
    set() { }
});

//keys 返回对象的键值
let objd = {name:'张山', age:18}
let a = Object.keys(objd)   //["name","age"]

//values 返回对象的值
let b = Object.values(objd) //["张山",18]

// entries 返回二维数组，对应的键值和值
let c = Object.entries(objd) //[["name","张山"],["age",18]]




// 判断空集合
Object.keys(obj).length === 0 && obj.constructor === Object  // true表示为空对象


```
