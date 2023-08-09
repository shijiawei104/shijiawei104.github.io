---
title: vue3+elementplus在el-form中使用@keyup.enter.native回车事件后，路径会多出一个问号
date: 2023-08-09 09:30:27
tags:
---

#### 这是因为在 form 中使用@keyup.enter.native 默认提交该表单，要想阻止该事件，需要在 el-form 上添加@submit.native.prevent 即可

```js
 <el-form @submit.native.prevent ref="searchFormRef" :inline="true" :model="queryFormData">
        <el-row>
          <el-col :span="24">
            <el-form-item prop="keyword" label="信息">
              <el-input  v-model="queryFormData.keyword" @keyup.enter.native="handleSearch" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" :icon="Search" @click="handleSearch">查询</el-button>
              <el-button :icon="Refresh" @click="resetSearch">重置</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
```
