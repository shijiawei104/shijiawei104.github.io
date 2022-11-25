---
title: ucharts图表设置横向滚动
date: 2022-11-25 09:24:38
tags:
---
### 使用ucharts图表时，当x轴数据过多，就需要设置横向滚动显示
#### 1.官网查询设置enableScroll
`enableScroll: true,`
#### 2.还要在组件设置ontouch属性，不然不生效
```html
<qiun-data-charts type="line" :opts="opts" :chartData="chartData" :canvas2d="true"
canvasId="xtSOZaayxsegBsGEZvmwpoRAvttZwisCa" :ontouch="true"/>
```
