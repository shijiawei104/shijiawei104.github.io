---
title: vue导出pdf
date: 2022-11-30 13:48:28
categories:
- vue
tags:
---
```js
//第一个：将页面html转换成图片
npm install --save html2canvas
//第二个:将图片生成pdf
npm install jspdf --save
// 引入
import html2Canvas from 'html2canvas'
import JsPDF from 'jspdf'
 /** 导出pdf */
      handleExportPdf() {
        var xname =  '名称'
        this.$nextTick(() => {
          // 生成的pdf只有页面窗口可见的区域，有滚动条的下面没有生成出来，需在生成PDF前，改overflow属性auto为visible
          // 获取dom高度、宽度
          var shareContent = document.querySelector('#out-table');
          var width = shareContent.offsetWidth / 4;
          var height = shareContent.offsetHeight / 4;
          html2Canvas(document.getElementById('out-table'), {
            dpi: 900,
            // scrolly: 0,
            // width:eleW,//生成后的宽度
            // height:eleH,//生成后的高度
            // scrollx: -10,
            useCORS: true, //允许canvas画布内可以跨域请求外部链接图片, 允许跨域请求。
  
            // backgroundColor: null //避免图片有白色边框
          }).then((canvas) => {
            var context = canvas.getContext('2d');
            context.mozImageSmoothingEnabled = false;
            context.webkitImageSmoothingEnabled = false;
            context.msImageSmoothingEnabled = false;
            context.imageSmoothingEnabled = false;
            var pageData = canvas.toDataURL('image/jpeg', 1.0);
            var img = new Image();
            img.src = pageData;
            img.onload = () => {
              // 获取dom高度、宽度
              img.width = img.width / 2;
              img.height = img.height / 2;
              img.style.transform = 'scale(0.5)';
              if (width > height) {
                // 此可以根据打印的大小进行自动调节
                // eslint-disable-next-line
                var pdf = new JsPDF('l', 'mm', [width * 0.505, height * 0.545]);
              } else {
                // eslint-disable-next-line
                var pdf = new JsPDF('p', 'mm', [width * 0.505, height * 0.545]);
              }
              pdf.addImage(pageData, 'jpeg', 0, 0, width * 0.505, height * 0.545);
              pdf.save(xname + '.pdf');
            };
          });
        });
  
      },
      ```