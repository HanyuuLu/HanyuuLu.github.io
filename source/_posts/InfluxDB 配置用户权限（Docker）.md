---
title: InfluxDB 配置用户权限（Docker）
date: 2021-01-20
tags:
  - Docker
  - InfluxDB
---

## 配置InfluxDB容器

1.  拉取镜像

镜像地址：https://hub.docker.com/_/influxdb

``` bash
docker pull influxdb
```

>   确认镜像存在
>
>   ``` bash
>   # docker image ls
>   REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
>   influxdb     latest    0454d5d215cc   7 days ago   307MB
>   ```

2.  运行

 ``` bash
docker run -d --name influxDB -p 8086:8086 influxdb:latest
 ```

>   确认容器存活
>
>   ``` bash
>   # docker container ls
>   CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS         PORTS                    NAMES
>   **********   influxdb   "/entrypoint.sh infl…"   48 minutes ago   Up 8 minutes   0.0.0.0:8086->8086/tcp   influxdb
>   ```

**InfluxDB远程访问默认不需要权限校验，在生产环境中显然不合适，下面配置用户权限**

## 配置用户权限

``` bash
# 进入容器
# docker exec -it influxdb /bin/bash
root@07a5ee195563:/# influx
Connected to http://localhost:8086 version 1.8.3
InfluxDB shell version: 1.8.3
#创建用户并赋予所有权限
> create user "admin" with password 'p@ssword'
> grant all privileges to admin
```

## 启用权限验证

``` bash
root@07a5ee195563:/# echo /etc/influxdb/influxdb.conf
/etc/influxdb/influxdb.conf
root@07a5ee195563:/# cat /etc/influxdb/influxdb.conf
[meta]
  dir = "/var/lib/influxdb/meta"

[data]
  dir = "/var/lib/influxdb/data"
  engine = "tsm1"
  wal-dir = "/var/lib/influxdb/wal"

[http]
  auth-enabled = true
```

*   重启服务即可

``` bash
docker restart influxDB
```



## 常用命令参考

```bash
# 显示用户
SHOW USERS
# 创建用户
CREATE USER "username" WITH PASSWORD 'password'
# 赋予用户管理员权限
GRANT ALL PRIVILEGES TO username
# 创建管理员权限的用户
CREATE USER <username> WITH PASSWORD '<password>' WITH ALL PRIVILEGES
# 修改用户密码
SET PASSWORD FOR username = 'password'
# 撤消权限
REVOKE ALL ON mydb FROM username
# 查看权限
SHOW GRANTS FOR username
# 删除用户
DROP USER "username"
```

