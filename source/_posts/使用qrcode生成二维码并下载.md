---
title: 使用qrcode生成二维码并下载
date: 2023-06-16 15:23:52
tags:
---

```js
import QrcodeVue from "qrcode.vue";

  <el-dialog v-model="dialogVisibleH5" title="二维码" width="25%">
        <div class="dialog-box">
          <qrcode-vue :size="240" :value="wxauthUrl" id="qrcode" />
          <p>请使用微信扫一扫</p>
          <el-button type="primary" size="small" @click="download">下载二维码图片</el-button>
        </div>
    </el-dialog>

 const download = () => {
  const qrcodeCanvas: any = document.getElementById("qrcode")
  if (qrcodeCanvas) {
    onDownload(qrcodeCanvas.toDataURL("image/png"), "微信上课提醒二维码")
  }
}

/** 下载事件 */
export const onDownload = (src: any, title: string) => {
  const href = isHttp(src) ? src : base64Img(src)
  const a = document.createElement("a")
  a.download = title
  // 设置图片地址
  a.href = href
  a.click()
  a.remove()
}
/** 补齐base图片格式 */
export const base64Img = (img: any) => {
  if (img.indexOf("data:image") < 0) {
    return "data:image/png;base64," + img
  } else {
    return img
  }
}
```
