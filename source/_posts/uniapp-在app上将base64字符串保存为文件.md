---
title: uniapp 在app上将base64字符串保存为文件
date: 2025-03-21 13:37:39
tags:
---

```js
将base64字符串保存为图片、Word、Excel、音频、视频等文件,支持Android、iOS
// base64:base64字符串
// fileName:文件名
// callback:回调函数，返回文件路径
base64ToFile(base64, fileName, function (path) {
    console.log('result', path)
    plus.runtime.openFile(path) //用第三方程序打开文件
})

function blobToBase64(blob) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.onerror = reject
        reader.onload = () => {
            if (typeof reader.result === 'string') {
                // 去掉"data:..."前缀，只保留 Base64 部分
                resolve(reader.result.split(',')[1])
            } else {
                reject('Unexpected result type')
            }
        }
        reader.readAsDataURL(blob)
    })
}

function base64ToFile(base64Str, fileName, callback) {
    // 去除base64前缀
    var index = base64Str.indexOf(',')
    var base64Str = base64Str.slice(index + 1, base64Str.length)

    plus.io.requestFileSystem(plus.io.PRIVATE_DOC, function (fs) {
        fs.root.getFile(
            fileName,
            {
                create: true
            },
            function (entry) {
                // 获得本地路径URL，file:///xxx/
                var fullPath = entry.fullPath
                let platform = uni.getSystemInfoSync().platform
                if (platform == 'android') {
                    var Base64 = plus.android.importClass('android.util.Base64')
                    var FileOutputStream = plus.android.importClass('java.io.FileOutputStream')
                    try {
                        var out = new FileOutputStream(fullPath)
                        // 此处Base64.decode有长度限制，如果不能满足需求，可以考虑换成官方原生插件市场的【Base64转文件】
                        var bytes = Base64.decode(base64Str, Base64.DEFAULT)
                        out.write(bytes)
                        out.close()
                        // 回调
                        callback && callback(entry.toLocalURL())
                    } catch (e) {
                        console.log(e.message)
                    }
                } else if (platform == 'ios') {
                    var NSData = plus.ios.importClass('NSData')
                    var nsData = new NSData()
                    nsData = nsData.initWithBase64EncodedStringoptions(base64Str, 0)
                    if (nsData) {
                        nsData.plusCallMethod({
                            writeToFile: fullPath,
                            atomically: true
                        })
                        plus.ios.deleteObject(nsData)
                    }
                    // 回调
                    callback && callback(entry.toLocalURL())
                }
            }
        )
    })
}
```
