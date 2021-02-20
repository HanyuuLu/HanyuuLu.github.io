---
title: 低成本个人NAS搭建解决方案（带内网穿透）
date: 2019-12-10 00:00:00
tags:
	- nas
	- raspberrypi
	- network
categories:
	- programming
---



# Hardware

* PC 或者 Raspberrypi 3b+ 一台
* 周边设备（键盘、鼠标、树莓派的储存卡之类）
* 可用的网络链接（内网有内网穿透方案）

# Software

* 操作系统下载
  * x86/x86_64
    * [windows](https://www.microsoft.com/zh-cn/software-download/windows10)
    * [deepin](https://www.deepin.org/)
    * [ubuntu](https://ubuntu.com/)
  * armv7 (Raspberry 3b(+)/4b)
    * [raspbian](https://www.raspberrypi.org/downloads/raspbian/)
> 搭建NAS可以使用server版本，机子有其他用途也可以使用其他版本
# Step
> 以Raspberry 3b+、Raspbian Buster with desktop、Nignx、Seafile Community组合为例。
1. 安装操作系统，安装流程参见各下载链接的官方安装文档
2. 测试网络连接
3. 国内用户使用Luiux系统请替换源以加速后续安装。
   * [tuna](https://mirrors.tuna.tsinghua.edu.cn/)
   > 找到对应的发行版然后戳右边问号小圆圈即可跳转到换源页面,比如raspbian的换源页面为[https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)
4. 安装[seafile](https://www.seafile.com/home/)服务
   1. 跳转到[下载](https://www.seafile.com/download/)页面选择对应的服务器端程序，[raspberry服务器端链接](https://github.com/haiwen/seafile-rpi/releases)，下载最新的release的.tar.gz文件（无gui环境可以使用wget下载）
   2. 参照seafile的[官方教程:部署Seafile 服务器（使用SQLite）](https://cloud.seafile.com/published/seafile-manual-cn/deploy/using_sqlite.md)进行安装，我们假定你并未更改默认的端口号（file端口8082,hub端口8000,若更改了请自行在后续说明中自行对应即可)
   3. 启动seafile测试能在localhost上访问到seafile服务   
   `http://localhost:8000'
5. 安装Nginx服务
    `sudo apt install nginx`安装完后启动Nginx(apt装完后直接启动) `http://localhost`能访问到Nginx的默认页面
6. 将Nginx服务请求转接到seafile上
    编辑`/etc/nginx/site-enabled/default`或者Nginx目录下的nginx.conf文件(windows)，建立服务器配置，将根节点转接到hub端口(8000)，将/server节点转接到file端口(8082)，参考如下
```
  server {
        listen 80 default_server;	# 监听在80端口，ipv4
        listen [::]:80 default_server;	# 监听在80端口, ipv6
        location / {	# 节点
                proxy_pass http://127.0.0.1:8000/;	# 转发端口
                client_max_body_size 8000m;	# 请求体大小限制（决定上传下载文件大小上限）
        }
        location /server/ {
                proxy_pass http://127.0.0.1:8082/;
                client_max_body_size 8000m;
        }
}
```
7. 在seafile上进行设置
   1. 管理员账户登陆，选择系统设置，设置，URL
   2. 设置SERVICE_URL和FILE_SERVER_ROOT为`http://[计算机名]`和`http://[计算机名]/server`（树莓派计算机名默认为raspberrypi）
8. 做内网穿透（可选）
   1. 使用[zerotier](https://www.zerotier.com/)，新建一个账号并创建一个网络
   2. 在服务器上安装zerotier，[下载并安装客户端](https://www.zerotier.com/download/)   
   3. 加入网络（gui直接戳戳按钮，zerotier-cli请参阅[zerotier-cli](https://zerotier.atlassian.net/wiki/spaces/SD/pages/29065282/zerotier-cli)）
# Finish
[]~(￣▽￣)~*
使用设备只需要安装zerotier并加入对应网络即可通过访问`http://[服务器计算机名]`即可

   

   

