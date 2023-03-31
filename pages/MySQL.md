type:: [[Database]]
language:: [[CPP]] 
category:: [[SQL]], [[OLTP]]

- Installation
	- [[Installing MySQL on Linux Using RPM Packages]]
- InnoDB
	- 文件
		- 日志文件
			- binlog
				- [[gtid]]
	- 锁
		- 锁机制
			- [[FTWRL和set global read_only=on的区别]]
		- 阻塞
			- [[MySQL 行锁等待超时]]
		- 死锁
			- [pt-deadlock-logger](https://blog.csdn.net/weixin_38708177/article/details/119999918)
	- 索引
		- [[MySQL索引详解]]
	- 事务
		- 事务的实现
			- redo
				- [数据存储 - 我们常听到的WAL到底是什么 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000022512468)
		- [[认识事务]]
- [[MySQL Backup and Recovery]]
	-
	- [[xtrabackup备份docker中mysql]]
	- [[Point-in-Time Recovery Using Binary Log]]
	- [[Use the MySQLBinlog tool for location-based or point-in-time data recovery]]
	- [MySQL 时间点恢复的正确方式 - Percona 数据库性能博客](https://www.percona.com/blog/2017/10/23/mysql-point-in-time-recovery-right-way/)
	- [[检查MySQL数据一致性]]
- Tools
	- [[Sysbench]]
- Courses
	- [[SQL进阶教程-Mosh]] #learning
	  :LOGBOOK:
	  CLOCK: [2022-08-24 Wed 22:52:38]--[2022-08-24 Wed 22:52:41] =>  00:00:03
	  CLOCK: [2022-12-07 Wed 20:56:42]--[2022-12-07 Wed 20:56:43] =>  00:00:01
	  CLOCK: [2023-01-08 Sun 21:23:39]--[2023-01-08 Sun 21:23:39] =>  00:00:00
	  CLOCK: [2023-01-08 Sun 21:23:41]--[2023-01-08 Sun 21:23:42] =>  00:00:01
	  :END:
	  [[高并发高性能高可用MySQL实战]]
	- [Schedule - CMU 15-721 :: Database Systems (Spring 2016)](https://15721.courses.cs.cmu.edu/spring2016/schedule.html)
	- [课程中心 (pingcap.com)](https://learn.pingcap.com/learner/course)
- 云原生
	- [RDS MySQL 云原生架构实践-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/785477)
	- [[Velero备份MySQL]]
	- [[Stash备份MySQL]]
- cluster
	- [MySQL Galera Cluster全解析 Part 3 状态快照传输(SST) - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1681426)