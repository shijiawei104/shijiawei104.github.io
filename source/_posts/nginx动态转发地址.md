---
title: nginx动态转发地址
date: 2026-02-26 14:20:31
tags:
---
### 有时会有需要根据请求参数动态转发到不同的后端地址的场景，比如根据不同的参数值转发到不同的服务器。
``` js
1.在前端cookie中设置一个参数，比如target，用来匹配不同的后端地址。

2.在nginx定义 Map，从 Cookie 中获取 target 参数值。
map $cookie_target $target_ip {
    "252"   192.168.1.252;
    "46"    192.168.1.46;
    "254"   192.168.1.254;
    default 192.168.1.252;
}
server {
    listen 8088;
    
    location /ISAPI/ {
        # 转发到 Map 映射出来的 IP
        proxy_pass http://$target_ip/ISAPI/;

        # ... 其他配置
    }
}
```