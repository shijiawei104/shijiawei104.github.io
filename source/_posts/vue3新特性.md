---
title: vue3新特性
date: 2022-11-23 21:53:13
tags:
---
```js
### 1.vue2：
objec.defineProperty()
不能监听数组的变化，
必须遍历对象的每一个属性

vue3:
Proxy
不需要遍历

### 2.setup语法糖插件：unplugin-auto-import
解决场景:在组件中开发无需每次引入import
下载安装：npm i unplugin-auto-import -D
配置：vivt.config.js中
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'

// https://vitejs.dev/config/
export default defineConfig({
plugins: [
vue(),
AutoImport({
imports:['vue','vue-router'] //自动导入vue和vue-router相关函数
})
]
})

### 3.toRefs:进行数据解构
### 4.
监听一个
watch(msg, (newVal,oldVal)=>{
    console.log(newVal)        
},{
    //初始值
    immediate:true
})
多个
watch([msg,str], (newVal,oldVal)=>{
    console.log(newVal)        
},{
    //初始值
    immediate:true
})
监听“对象”中某个对象
watch(() => obj.arr, (newVal,oldVal)=>{
    console.log(newVal)        
})
立即执行监听函数
watchEffect(()=>{
    console.log(msg.value)
})
### 5.生命周期
vue2:选项式API beforeCreate、created
vue3:setup 组合式API
### 6.路由
useRoute =>this.$route
useRouter =>this.$router
### 7.父传子
父组件： import引入子组件后不用commponts
        let msg = ref('这是父传过去的数据')
子组件:  1.setup语法糖
        import {defineProps} from 'vue'
        defineProps({
            msg:{
                type:String,
                default:'1'
            }
        })
        2.选项式API
        props:['msg'],
        setup(props){
            let obj = props.msg
            return{
                obj
            } 
        }
### 8.子传父
子组件：
    1.组合式
     let num = ref(100)
     const emit = defineEmits(['fn'])
    const changeNum=()=>{
        emit('fn',num)
    }
    2.选项式
    setup(props,{emit}){
        let num = ref(100)
        const changNum=()=>{
            emit('fn',num)
        }
        return{
            num,
            changeNum
        }
    }
父组件:
<List @fn="changeHome" />
let changeHome=(n)=>{
    console.log(n.value)    
}

### 9.父子组件双向数据
父组件：<List  v-model:num='num' />
        let num = ref(1)
子组件：const props=defineProps({
        num:{
            type: Number,
            default: 100,
        }
    })
        const emit=defineEmits(['update:num'])
        const btn(()=>{
            emit('uodate:num',200)
        })

### 10.插槽
匿名插槽 
具名插槽: 父：v-slot:xxx(简写：#xxx)  子：<slot name:xxx></slot> 
作用域插槽：内容包裹在一个拥有单个参数的函数里  动态插槽:绑定动态数据，可以进行切换

### 11.Teleport:传送内容到具体标签里面（必须有这个dom的内容）
<teleport to='#container'></teleport>
<teleport to='.main'></teleport>
<teleport to='body'></teleport>

### 12.动态组件：
<component :is="动态去切换组件" />
```