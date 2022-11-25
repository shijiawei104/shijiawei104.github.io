---
title: websocket结合ftv.js实现视频直播流播放
date: 2022-11-25 08:45:40
tags:
---

1.安装 `websocket： npm i reconnecting-websocket`
  安装 `ftv.js：npm i flv.js -S`
2.引入 
       `import ReconnectingWebSocket from 'reconnecting-websocket';`
       `import flvjs from "flv.js";`
#### 3.
    ```html
    <video  width="100%" height="100%" ref="videosmallone" controls  preload="auto" muted  autoplay type="rtmp/flv">
            <source src="" />
    </video>
    ```
  
#### 4. 先websocket连接，与服务器建立实时通讯，当前端视频页面关闭时，后端可以实时接收，避免前端已经关闭视频流，后端没有及时接收还在一直传输，影响服务器性能
```js
      // 创建 websocket 链接
      createWebsocket(id) {
      let httpURL = process.env.VUE_APP_BASE_API  
      let url = `ws://${httpURL}/wsMonitoring/${id}`
      this.websocket = new ReconnectingWebSocket(url)
      // 连接发生错误的回调方法
      this.websocket.onerror = this.websocketOnerror;
      // 连接成功建立的回调方法
      this.websocket.onopen = this.websocketOnopen;
      // 接收到消息的回调方法
      this.websocket.onmessage = this.websocketOnmessage;
      // 连接关闭的回调方法
      this.websocket.onclose = this.websocketOnclose;
      // 监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
      this.websocket.onbeforeunload = this.websocketOnbeforeunload;
    },
    // 连接发生错误的回调方法
    websocketOnerror() {
      console.log('连接发生错误的回调方法');
    },
    // 连接成功建立的回调方法
    websocketOnopen() {
      console.log('连接成功建立的回调方法');
    },
    // 接收到消息的回调方法
    websocketOnmessage(event) {
      console.log('接收到消息的回调方法', event.data);
    },
    // 连接关闭的回调方法
    websocketOnclose() {
      console.log('连接关闭的回调方法');
    },
    // 监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常
    websocketOnbeforeunload() {
      this.closeWebSocket();
      console.log('监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常');
    },
    // 关闭WebSocket连接
    closeWebSocket() {
      this.websocket.close();
    },
    
```
#### 5.视频流播放
```js
    // ftv.js 视频播放 
    init(val) {
      this.$nextTick(()=>{
        var videoElement = this.$refs.videosmallone; // 获取到html中的video标签
        if (flvjs.isSupported()) {
        //进来先判断 player是否存在，如果存在，销毁掉它，不然会占用TCP名额
          if (this.player !== null) {
            this.player.pause();
            this.player.unload();
            this.player.detachMediaElement();
            this.player.destroy();
            this.player = null;
          }
          this.player = flvjs.createPlayer( //创建直播流，加载到DOM中去
            {
              type: "flv",
              url: val, //你的url地址
              isLive: true, //数据源是否为直播流
              hasAudio: false, //数据源是否包含有音频
              hasVideo: true, //数据源是否包含有视频
              enableStashBuffer: true, //是否启用缓存区
            },
            {
              enableWorker: false, //不启用分离线程
              enableStashBuffer: false, //关闭IO隐藏缓冲区
              autoCleanupSourceBuffer: true, //自动清除缓存
              lazyLoad: false,
            }
          );
          this.player.attachMediaElement(videoElement); //放到dom中去
          this.player.load();//准备完成
          //!!!!!!这里需要注意，有的时候load加载完成不一定可以播放，要是播放不成功，用settimeout 给下面的this.player.play() 延时几百毫秒再播放
            this.player.play();//播放    
        }
      })
    },
```