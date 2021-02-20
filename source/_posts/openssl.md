---
title: 使用OpenSSL生成SSL数字认证以用于Nginx的HTTPS通信
date: 2020-03-22 00:00:00
categories: 
    - programming
tags:
    - SSL
    - HTTPS
    - Nginx
---
* 生成私钥

  ` openssl genrsa -out private.pem 2048`

* 利用生成的私钥生成公钥

  `openssl req -x500 -key private.pem -out cert.pem -days 3650`

* 修改配置文件

  ```
  server {
  	listen 443;    
  	ssl on;    
  	ssl_certificate /etc/ssl/cacert.pem;        
  	# path to your cacert.pem    
  	ssl_certificate_key /etc/ssl/privkey.pem;    
    # path to your privkey.pem    
    server_name seafile.example.com;    
    server_tokens off;    
    # ......    
    proxy_pass http://127.0.0.1:8000;    
    proxy_set_header   Host $host;    
    proxy_set_header   X-Real-IP $remote_addr;    
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;    
    proxy_set_header   X-Forwarded-Host $server_name;    
    proxy_set_header   X-Forwarded-Proto https;
    proxy_read_timeout  1200s;
  }
  ```

* 强制将http请求转为https请求

  ```
  server {
          listen  80;
          listen  [::]:80;
          rewrite ^ https://$http_host$request_uri? permanent;
          server_tokens off;
  }
  ```

  