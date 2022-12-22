---
title: echarts图表自适应和其他问题
date: 2022-12-22 15:51:27
tags:
---

### 1.使用 eCharts 提供的 resize()方法，监听图表容器的大小并改变图表大小

```js
// 监听 resize 事件
window.addEventListener("resize", function () {
  this.chart.resize();
});
```

报错如下：
`Uncaught TypeError: Cannot read properties of undefined (reading 'resize')`

解决：
主要是 this 指向的问题，换成箭头函数即可解决：

```js
window.addEventListener("resize", () => {
  this.chart.resize();
});
```

报错二：
`TypeError: Cannot read properties of undefined (reading 'getAttribute')`
解决：
主要原因是离开当前页面没有清除自适应事件：

```js
destroyed () {
    window.removeEventListener('resize', this.chart)
},
```

### 2.echarts 图表无数据时展示"暂无数据"

```js
<div id="test_chart" style="width: 600px;height:400px;"></div>;

let chartData = [5, 20, 36, 10, 10, 20];
// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById("test_chart"));
var option;
// 指定图表的配置项和数据
if (chartData.length == 0) {
  //暂无数据
  option = {
    title: {
      text: "暂无数据",
      x: "center",
      y: "center",
      textStyle: {
        fontSize: 14,
        fontWeight: "normal",
      },
    },
  };
} else {
  option = {
    title: {
      text: "ECharts 入门示例",
    },
    tooltip: {},
    legend: {
      data: ["销量"],
    },
    xAxis: {
      data: ["衬衫", "羊毛衫"],
    },
    yAxis: {},
    series: [
      {
        name: "销量",
        type: "bar",
        data: chartData, //动态数据
      },
    ],
  };
}
// 使用刚指定的配置项和数据显示图表。
myChart.setOption(option);
```

### 3.vue 使用 element-resize-detector 监听元素宽度变化来实现 echarts 的自适应

原文：https://blog.csdn.net/weixin_44090040/article/details/120910088
当我们切换左侧菜单展示效果的时候，右侧内容会对应变宽，但此时的 echarts 并不能执行自适应效果，这是因为切换菜单展示效果并没有触发 window.onresize，所以为解决类似此问题，我们可使用 element-resize-detector。

```js
npm install element-resize-detector --save
```

创建文件 chartResize.js

```js
import echarts from "echarts";
import Vue from "vue";
import elementResizeDetectorMaker from "element-resize-detector";

export var version = "0.0.1";
var compatible = /^2\./.test(Vue.version);
if (!compatible) {
  Vue.util.warn(
    "vue echarts resize directive " +
      version +
      " only supports Vue 2.x, and does not support Vue " +
      Vue.version
  );
}
let HANDLER = "_vue_echarts_resize_handler";

function bind(el) {
  unbind(el);
  el[HANDLER] = function () {
    let chart = echarts.getInstanceByDom(el);
    if (!chart) {
      return;
    }
    chart.resize();
  };
  //监听window窗体变化，更新echarts大小
  //window.addEventListener("resize", el[HANDLER])
  //监听绑定的div大小变化，更新echarts大小
  elementResizeDetectorMaker().listenTo(el, el[HANDLER]);
}
function unbind(el) {
  //window.removeEventListener("resize", el[HANDLER]);
  elementResizeDetectorMaker().removeListener(el, el[HANDLER]);
  delete el[HANDLER];
}
var directive = {
  bind: bind,
  unbind: unbind,
};
Vue.directive("on-echart-resize", directive);
```

在页面中使用

```js
<template>
  <div>
 	 <div id="contentShow" ref="lineChart" v-on-echart-resize></div>
  </div>
</template>
<script>
import "../../../utils/chartResize";
export default {
  data() {
    return {
    };
  },
  mounted() {
    const elementResizeDetectorMaker = require("element-resize-detector");
    let erd = elementResizeDetectorMaker();
    erd.listenTo(this.$refs.lineChart, () => {
      this.$nextTick(function () {
        const lineChart = this.$echarts.init(this.$refs.lineChart);
        //使echarts尺寸重置
        lineChart.resize();
      });
    });
  },
  methods: {}
};
</script>
<style scoped>
</style>
```
