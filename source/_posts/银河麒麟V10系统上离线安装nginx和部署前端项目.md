---
title: 银河麒麟V10系统上离线安装nginx和部署前端项目
date: 2025-05-22 15:43:41
tags:
---

在国产化系统银河麒麟安装 nginx 需要安装三个
依赖组件：openssl，zlib，pcre
链接：https://pan.baidu.com/s/1IY-KERu7uimVS8oBFpFmAQ?pwd=y0id
提取码：y0id
一、安装 nginx
将 nginx 及其依赖上传至目标服务器

1.  安装 openssl

    解压 openssl-3.0.3.tar.gz

    ```js
    tar -zxvf openssl-3.0.3.tar.gz
    ```

    进入 openssl-3.0.3/目录

    ```js
    cd openssl-3.0.3/
    ```

    编译安装

    ```js
    ./config && make && make install
    ```

2.安装 zlib
解压 zlib-1.2.12.tar.gz

```js
tar -zxvf zlib-1.2.12.tar.gz
```

进入 zlib-1.2.12/目录

```js
cd zlib-1.2.12/
```

编译安装

```js
./configure && make && make install

```

3. 安装 pcre
   解压 pcre2-10.40.tar.gz

    ```js
     tar -zxvf pcre2-10.40.tar.gz
    ```

    进入 pcre2-10.40/目录

```js
cd pcre2-10.40/
```

编译安装

```js
./configure && make && make install
```

4. 安装 nginx
   解压 nginx-1.21.6.tar.gz

```js
tar -zxvf nginx-1.21.6.tar.gz
```

进入 cd nginx-1.21.6/目录

```js
cd nginx-1.21.6/
```

编译安装（默认安装到/usr/local 目录下,或者自定义安装位置./configure --prefix=/opt/myapp）

```js
./configure && make && make install
```

二、启动 nginx
进入 /usr/local/nginx/sbin 目录

```js
cd / usr / local / nginx / sbin
```

启动服务

```js
./nginx
```

停止服务

```js
./nginx -s stop
```

重启服务

```js
./nginx -s reload
```

启动成功后浏览器输入 localhost 显示 Welcome tonginx! 即安装成功！

三、前端部署
修改 nginx 配置文件

```js
vim / usr / local / nginx / conf / nginx.conf
```

找到这个地方

````js
listen       80;
server_name  localhost;
```
更改为自己所需部署的参数(root后面是前端dist包所在位置，proxy_pass后面是后端服务地址)
```js

server {
listen 80;
server_name localhost;
location / {
root /usr/local/nginx/html/dist;
try_files $uri $uri/ /index.html last;
index index.html;
}
location /prod-api/ {
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header REMOTE-HOST $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_pass http://192.168.0.1:8888/;
}
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
}
```

````

最后刷新 nginx 配置
./nginx -s reload
