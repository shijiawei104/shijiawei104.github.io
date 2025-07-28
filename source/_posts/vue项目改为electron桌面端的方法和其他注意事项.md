---
title: vue项目改为electron桌面端的方法和其他注意事项
date: 2025-07-28 15:39:00
tags:
---

### 1.在项目根目录运行

vue add electron-builder
如果报错找不到 vue 命令，需要先安装 vue cli:npm install -g @vue/cli

### 2.安装完成后在 package.json 里面会出现几条新命令：

执行 npm run electron:serve 打开桌面端应用
执行 npm run electron:build 打包桌面端应用

### 修改打包后的 exe 图标和名称

1.首先安装图标生成器：npm i electron-icon-builder

2.在 package.json 的 scripts 中添加一条命令并保存：
"electron:generate-icons": "electron-icon-builder --input=./public/icon.png --output=build --flatten"
修改应用名称添加"productName":"应用名称"

3.将你准备的图片命名为 icon.png 并复制到 public 目录下：

4.最后执行命令：npm run electron:generate-icons
生成的一系列图片在项目中多出来的 build 目录下的 icons 下：

5.删除 dist_electron 文件夹，重新执行打包命令，npm run electron:build

6.打包完成后 exe 图标和名称就会改变

### 修改桌面应用左上角的菜单显示，electron 默认为英文显示，如果需要更改或者删除需要在 background.js 文件中修改

```js
import { app, BrowserWindow,  Menu } from 'electron'

function setChineseMenu() {
  const template = [
    {
      label: '文件',
      submenu: [
        { label: '新建窗口', accelerator: 'CmdOrCtrl+N' },
        { label: '关闭', accelerator: 'CmdOrCtrl+W', role: 'close' }
      ]
    },
    {
      label: '编辑',
      submenu: [
        { label: '撤销', accelerator: 'CmdOrCtrl+Z', role: 'undo' },
        { label: '重做', accelerator: 'Shift+CmdOrCtrl+Z', role: 'redo' },
        { type: 'separator' },
        { label: '剪切', accelerator: 'CmdOrCtrl+X', role: 'cut' },
        { label: '复制', accelerator: 'CmdOrCtrl+C', role: 'copy' },
        { label: '粘贴', accelerator: 'CmdOrCtrl+V', role: 'paste' },
        { label: '全选', accelerator: 'CmdOrCtrl+A', role: 'selectAll' }
      ]
    },
    {
      label: '视图',
      submenu: [
        { label: '刷新', accelerator: 'CmdOrCtrl+R', click: () => { BrowserWindow.getFocusedWindow().reload() } },
        { label: '开发者工具', accelerator: 'CmdOrCtrl+Shift+I', click: () => { BrowserWindow.getFocusedWindow().webContents.openDevTools() } }
      ]
    },
    {
      label: '帮助',
      submenu: [
        { label: '关于', click: () => { /* 打开关于窗口 */ } }
      ]
    }
  ]

  async function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: process.env.ELECTRON_NODE_INTEGRATION,
      contextIsolation: !process.env.ELECTRON_NODE_INTEGRATION
    }
  })

  setChineseMenu()
}
```

### 注意事项：

electron 中使用 location.href='/index' 会白屏，应修改为： window.location.reload("/index");

electron 无法用 this.$refs.dom; 获取到 dom 元素，应改为： document.getElementById("dom");
