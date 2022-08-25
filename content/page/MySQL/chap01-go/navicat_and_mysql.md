---
title: 1-8 navicat and mysql
tags: 
- navicat
categories: 
- MySQL
date: 2022-05-20
lastMod: 2022-05-20
---
## 1 安装MySQL

### 1.1 docker安装MySQL

### 1.2 通过镜像启动

```bash
[root@yang ~]# docker run -p 3306:3306 --name mymysql -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7

-v $PWD/conf:/etc/mysql/conf.d: 将主机当前目录下的conf/my.cnf挂载到容器的/etc/mysql/my.cnf

-v $PWD/logs:/logs: 将主机当前目录下的logs目录挂载到容器的/logs

-v $PWD/data:/var/lib/mysql 将主机当前目录下的data目录挂载到容器的/var/lib/mysql

-e MYSQL_ROOT_PASSWORD=123456 初始化root用户的密码
```

### 1.3 进入容器配置

- 进入容器启动mysql

```bash
[root@yang ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                    PORTS                               NAMES
6fed3a0f193b        mysql:5.7           "docker-entrypoint.s…"   About an hour ago   Up About an hour          0.0.0.0:3306->3306/tcp, 33060/tcp   mymysql
d3cac0da1248        hello-world         "/hello"                 37 hours ago        Exited (0) 37 hours ago                                       determined_goldberg
[root@yang ~]# 
[root@yang ~]# docker exec -it 6fed3a0f193b /bin/bash
root@6fed3a0f193b:/# 
root@6fed3a0f193b:/# mysql -uroot -p123456
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.33 MySQL Community Server (GPL)
```

- 创建用户并授权

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'127.0.0.1' IDENTIFIED BY 'root' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'root' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

## 2 安装navicat

[http://www.navicat.com.cn/download/navicat-premium](http://www.navicat.com.cn/download/navicat-premium)

[https://www.bilibili.com/read/cv10270203](https://www.bilibili.com/read/cv10270203)