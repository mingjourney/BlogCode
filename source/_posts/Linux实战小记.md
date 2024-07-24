---
title: Nginx实战小记
date: 2024-7-23 10:47:33  
tags:
- Nginx
- 前端
categories: 
- 笔记
---

### 准备工作

```shell
yum -y install gcc gcc-c++ autoconf pcre-devel make automake
yum -y install wget httpd-tools vim

mkdir gugu
cd gugu
mkdir app backup download logs work

yum list | grep nginx

yum install nginx
nginx -v

```

### 了解nginx

```shell
rpm -ql nginx
# rpm 是linux的rpm包管理工具，-q 代表询问模式，-l 代表返回列表，这样就可以找到nginx的所有安装位置了。
```

cd /etc/nginx
vim nginx.conf

```json
#运行用户，默认即是nginx，可以不进行设置
user  nginx;
#Nginx进程，一般设置为和CPU核数一样
worker_processes  1;   
#错误日志存放目录
error_log  /var/log/nginx/error.log warn;
#进程pid存放位置
pid        /var/run/nginx.pid;


events {
    worker_connections  1024; # 单个后台进程的最大并发数
}


http {
    include       /etc/nginx/mime.types;   #文件扩展名与类型映射表
    default_type  application/octet-stream;  #默认文件类型
    #设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   #nginx访问日志存放位置

    sendfile        on;   #开启高效传输模式
    #tcp_nopush     on;    #减少网络报文段的数量

    keepalive_timeout  65;  #保持连接的时间，也叫超时时间

    #gzip  on;  #开启gzip压缩

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件


```



**查看当前进程**

```shell
ps aux | grep nginx
```

`ps`：显示当前运行中的进程。

`a`：显示与终端无关的所有进程。

`u`：以用户友好的格式显示进程的详细信息，包括用户、CPU 和内存使用情况等。

`x`：显示没有控制终端的进程。



**监听端口和相关进程**

```shell
netstat -tlnp
```

`-t`：显示 TCP 协议的连接。

`-l`：显示监听中的套接字。

`-n`：显示数字地址和端口号，而不是解析后的主机名和服务名。

`-p`：显示使用这些套接字的进程 PID 和名称（需要 root 权限）。



```shell
nginx -s reload

sudo nginx -s reload -t

```

```js
/i[\s\S]*?you/
/i.*?you/
```

```json
配置以域名为划分的虚拟主机
我们修改etc/nginx/conf.d目录下的default.conf 文件，把原来的80端口虚拟主机改为以域名划分的虚拟主机。代码如下：
 代码解读复制代码server {
    listen       80;
    server_name  nginx.gugu.com;
我们再把同目录下的8001.conf文件进行修改，改成如下：
 代码解读复制代码server{
        listen 80;
        server_name nginx2.gugu.com;
        location / {
                root /usr/share/nginx/html/html8001;
                index index.html index.htm;
        }
}

```

### 反向代理

现在我们要访问`http://nginx2.gugu.com`然后反向代理到`gugu.com`这个网站。我们直接到`etc/nginx/con.d/8001.conf`进行修改。

修改后的配置文件如下：

```
 代码解读
复制代码server{
        listen 80;
        server_name nginx2.gugu.com;
        location / {
               proxy_pass http://gugu.com;
        }
}
```

一般我们反向代理的都是一个IP，但是我这里代理了一个域名也是可以的。其实这时候我们反向代理就算成功了，我们可以在浏览器中打开`http://nginx2.gugu.com`来测试一下。（视频中有详细的演示）

**其它反向代理指令**

反向代理还有些常用的指令，我在这里给大家列出：

- proxy_set_header :在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
- proxy_connect_timeout:配置Nginx与后端代理服务器尝试建立连接的超时时间。
- proxy_read_timeout : 配置Nginx向后端服务器组发出read请求后，等待相应的超时时间。
- proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待相应的超时时间。
- proxy_redirect :用于修改后端服务器返回的响应头中的Location和Refresh

```json
server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
         root /usr/share/nginx/pc;
         if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
            root /usr/share/nginx/mobile;
         }
         index index.html;
        }
}

```

