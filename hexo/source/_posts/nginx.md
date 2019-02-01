---
title: 使用nginx解决前后端分离跨域
date: 2019-02-01 10:05:38
tags: nginx 服务器
categories: 工具 跨域
---
## nginx 使用为了解决跨域问题

windows开发环境可本地运行nginx服务监听对应svn路径下的html页面
即可将对应路径为 '/apis' 的ajax请求转发到 nginx.config 里 proxy_pass 对应的域名端口
以此避免跨域
### nginx for windows

#### 启动命令
``` js
start nginx // 启动nginx
nginx -s quit // 完整有序的停止nginx
nginx -s stop // 关闭nginx ：快速停止nginx
nginx -s reload // 修改配置后重新加载生效
```
#### 配置
``` js
sendfile        off; // 设置不缓存js
server {
        listen       3000;
        server_name  localhost;
        location / {
            root   F://项目文件//SVN_//抽奖//gzwh_Lottery_Front//trunk//lottery-page;
            index  index.html index.htm;
        }
        location /apis {
			      rewrite  ^/apis/(.*)$ /$1 break;
			      proxy_pass   http://localhost:8086;
        }
}
```
### nginx for linux
#### 安装及启动
``` js
yum -y install nginx // 安装nginx
rpm -ql nginx // 查看安装路径
service nginx start // 启动
service nginx stop // 停止
service nginx restart // 重启
```
#### 项目相关记录
``` js
workespace4 工作区
centos nginx.config 目录 /etc/nginx
// 使用svn
yum install -y subversion
svn checkout  http://svn.com/path
svn update
path: /root/project/gzwh_Lottery_Front/trunk/lottery-page
```

配置
``` js
user root; // 解决403问题
sendfile off; // 关闭js自动缓存
server {
  listen 3000 default_server;
  server_name localhost;
  location / {
    root /root/project/gzwh_Lottery_Front/trunk/lottery-page;
  }
  location /apis {
    rewrite ^/apis/(.*)$ /$1 break;
    proxy_pass http://localhost:8086;
  }
}
```
参考地址[yum 和 源码包 安装的 区别](https://segmentfault.com/a/1190000007116797)







