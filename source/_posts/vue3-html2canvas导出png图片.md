---
title: vue3+html2canvas导出png图片
date: 2023-06-19 11:32:07
tags:
---

```js
import html2canvas from "html2canvas";
const png = ref();
const exportPNG = () => {
  const refpng: any = document.getElementById("png");
  // html2canvas配置项
  const ops = {
    // 提高图片质量
    scale: 2,
    // 图片跨域
    useCORS: true,
    allowTaint: false,
    // 分辨率
    dpi: 300,
    // 解决导出的内容有横向滚动条
    width: refpng.scrollWidth,
    windowWidth: refpng.scrollWidth,
    // 解决导出的内容有竖向滚动条
    height: refpng.scrollHeight,
    windowHeight: refpng.scrollHeight,
  };
  html2canvas(png.value, ops).then((canvas) => {
    const imgUrl = canvas.toDataURL("image/png");
    const a = document.createElement("a");
    a.style.display = "none";
    a.href = imgUrl;
    a.setAttribute("download", "图片名称");
    if (typeof a.download === "undefined") {
      a.setAttribute("target", "_blank");
    }
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    window.URL.revokeObjectURL(imgUrl);
  });
};
```
