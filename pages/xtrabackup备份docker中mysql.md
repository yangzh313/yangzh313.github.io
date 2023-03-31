- 前提
	- docker中安装xtrabackup和mysql，使用xtrabackup对mysql发起备份和恢复
- 环境
	- 系统版本：CentOS Linux release 7.9.2009 (Core)
	- Mysql版本：5.7.36
	- Xtrabackup版本： 2.4.21
	- Docker版本： 20.10.22
- 架构
	- 宿主机 一台
	- Mysql容器 单节点
	- Xtrabackup容器 单节点
	- ![](https://img2020.cnblogs.com/blog/1007916/202110/1007916-20211012163137734-120460734.png)
- 容器部署注意点：
	- Mysql容器运行时，需要将数据目录（/var/lib/mysql）、配置文件（my.cnf）挂载到宿主机的目录上，并且Xtrabackup容器也做相同的目录挂载，这样就等于在宿主机上直接操作数据的备份与还原。
- 操作步骤
	- 宿主机新建mysql相关文件夹、文件
		- 创建/home/mysql5.7.36/conf/my.cnf
		  ```
		  [mysqld]
		  max_connections=1000
		  character-set-server=utf8
		  collation-server=utf8_general_ci
		  lower_case_table_names=1
		  sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
		  log-bin=mysql-bin
		  server-id=20808
		  
		  datadir=/var/lib/mysql
		  socket=/var/lib/mysql/mysql.sock
		  
		  # Disabling symbolic-links is recommended to prevent assorted security risks
		  symbolic-links=0
		  
		  log-error=/var/lib/mysql/mysqld.log
		  pid-file=/var/lib/mysql/mysqld.pid
		  ```
	- 启动mysql
		- 映射配置文件，挂载出mysql数据目录和日志目录
		  ```
		  2023-02-07 14:51:29 docker run --name mysql \
		  -p 3308:3306 \
		  -v /home/mysql5.7.36/conf/my.cnf:/etc/mysql/my.cnf \
		  -v /home/mysql5.7.36/data:/var/lib/mysql \
		  -e TZ=Asia/Shanghai \
		  -e MYSQL_ROOT_PASSWORD=* \
		  -d mysql:5.7
		  bb7ee804dcd5727e418f03de904a78329ec2de0f547f3cd71fba3382ed9b44f0
		  ```
		- docker logs mysql查看无报错日志
	- 新建Xtrabackup相关文件夹、文件
		- `mkdir /home/xtrabackup`
	- 安装xtrabackup并启动容器
		- ```
		  root@VM-16-15-centos mysql]# docker pull f763180872/xtrabackup
		  docker run -it -d \
		    --name xtrabackup \
		    --network=host \
		    -e TZ=Asia/Shanghai \
		    -v /home/backup:/backup \
		    -v /home/data:/data \
		    -v /home/mysql5.7.36/conf/my.cnf:/etc/mysql/my.cnf \
		    -v /home/mysql5.7.36/data:/var/lib/mysql \
		    f763180872/xtrabackup:latest
		  ```
	- 进入xtrabackup容器并发起备份和恢复
		- 全备
		  ```
		  [root@VM-16-15-centos mysql]# docker exec -it xtrabackup bash
		  	
		  xtrabackup --defaults-file=/etc/mysql/my.cnf \
		  --backup \
		  --datadir /var/lib/mysql \
		  --socket=/var/lib/mysql/mysql.sock \
		  --user=root \
		  --password=* \
		  --target-dir /data/full
		  ```
		- 增备
		  ```
		  root@VM-16-15-centos:/# xtrabackup --defaults-file=/etc/mysql/my.cnf --backup --datadir /var/lib/mysql --user=root --password=* --slave-info --target-dir /data/inc1 --socket=/var/lib/mysql/mysql.sock --incremental-basedir /data/full
		  root@VM-16-15-centos:/# ll /data
		  total 16
		  drwxr-xr-x 4 root root 4096 Feb  7 07:52 ./
		  drwxr-xr-x 1 root root 4096 Feb  7 07:25 ../
		  drwxr-x--- 5 root root 4096 Feb  7 07:43 full/
		  drwxr-x--- 5 root root 4096 Feb  7 07:52 inc1/
		  ```
		- 宿主机stop mysql容器，删除数据目录
		  ```
		  [root@VM-16-15-centos mysql5.7.36]# docker stop mysql
		  mysql
		  [root@VM-16-15-centos mysql5.7.36]# rm -rf /home/mysql5.7.36/data/
		  ```
		- xtrabackup发起恢复
		  ```
		  xtrabackup --target-dir /data/full \
		    --apply-log-only \
		    --prepare
		    
		    xtrabackup --target-dir /data/full \
		    --incremental-dir /data/inc1 \
		    --prepare
		    
		    xtrabackup --copy-back \
		    --datadir=/var/lib/mysql \
		    --target-dir=/data/full
		  ```
		- 启动mysql容器
		  ```
		  [root@VM-16-15-centos mysql5.7.36]# docker start mysql
		  mysql
		  
		  [root@VM-16-15-centos mysql5.7.36]# docker logs mysql
		  2023-02-07 15:01:03+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
		  2023-02-07 15:01:03+08:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
		  2023-02-07 15:01:03+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
		  2023-02-07 15:03:17+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
		  2023-02-07 15:03:18+08:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
		  2023-02-07 15:03:18+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
		  2023-02-07 16:38:17+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started.
		  2023-02-07 16:38:17+08:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
		  2023-02-07 16:38:17+08:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.36-1debian10 started
		  ```
-
- 参考
	- [xtrabackup对mysql5.7进行全备、还原的过程记录（docker版本） - Boom__Clap - 博客园 (cnblogs.com)](https://www.cnblogs.com/yourstars/p/15398103.html)
	- [docker mysql数据备份xtrabackup - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1748436)
-