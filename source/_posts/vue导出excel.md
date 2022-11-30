---
title: vue导出excel
date: 2022-11-30 13:42:37
categories:
- vue
tags:
---

```js
// 安装依赖
npm install --save xlsx file-saver
// 引入
import FileSaver from 'file-saver'
import XLSX from 'xlsx'
/** 导出表格 */
      handleExportExcel() {
        var xname = '表格名称.xlsx'
        var xlsxParam = { raw: true };
        /* 从表生成工作簿对象 */
        var wb = XLSX.utils.table_to_book(
          document.querySelector("#out-table"),
          xlsxParam
        );
        /* 获取二进制字符串作为输出 */
        var wbout = XLSX.write(wb, {
          bookType: "xlsx",
          bookSST: true,
          type: "array",
        });
        try {
          FileSaver.saveAs(
            //Blob 对象表示一个不可变、原始数据的类文件对象。
            //Blob 表示的不一定是JavaScript原生格式的数据。
            //File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。
            //返回一个新创建的 Blob 对象，其内容由参数中给定的数组串联组成。
            new Blob([wbout], { type: "application/octet-stream" }),
            //设置导出文件名称
            xname
          );
        } catch (e) {
          if (typeof console !== "undefined") console.log(e, wbout);
        }
        return wbout
      },
    ```