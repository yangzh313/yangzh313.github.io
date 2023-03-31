- description
	- ```
	  Lock wait timeout exceeded; try restarting transaction
	  ```
- reproduce
	- 插入数据
	  ```
	  mysql> use zy;
	  Database changed
	  mysql> delimiter $$
	  mysql> create procedure auto_insert() BEGIN     declare i int default 1;     while(i<1000000)do         insert into tmp values(i);         set i=i+1;     end while; END$$
	  Query OK, 0 rows affected (0.04 sec)
	  mysql> delimiter ;
	  
	  mysql> show create procedure auto_insert\G
	  *************************** 1. row ***************************
	             Procedure: auto_insert
	              sql_mode: STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
	      Create Procedure: CREATE DEFINER=`root`@`%` PROCEDURE `auto_insert`()
	  BEGIN     declare i int default 1;     while(i<1000000)do         insert into tmp values(i);         set i=i+1;     end while; END
	  character_set_client: utf8
	  collation_connection: utf8_general_ci
	    Database Collation: utf8mb4_general_ci
	  1 row in set (0.00 sec)
	  
	  mysql>
	  mysql> call auto_insert();
	  
	  ```
	- 删除行1，不提交
	  ```
	  mysql> start transaction;
	  Query OK, 0 rows affected (0.00 sec)
	  
	  mysql> delete from tmp where id = 1;
	  Query OK, 1 row affected (0.00 sec)
	  
	  ```
	- 删除id<1
	  ```
	  mysql> delete from tmp where id < 10;
	  ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
	  ```
- innodb monitor
	- shell
	- ```
	  [root@VM-16-15-centos scripts]# cat innodb_lock_monitor.sh
	  #!/bin/bash
	  
	  #账号、密码、监控日志
	  
	  user="root"
	  
	  password=*
	  
	  logfile="/root/resources/scripts/innodb_lock_monitor.log"
	  
	  while true
	  do
	          num=`mysql -u${user} -p${password} -e "select count(*) from information_schema.innodb_lock_waits" |grep -v count`
	          if [[ $num -gt 0 ]];then
	              date  >> /root/resources/scripts/innodb_lock_monitor.log
	              mysql -u${user} -p${password} -e  "SELECT r.trx_mysql_thread_id waiting_thread,r.trx_query waiting_query, \
	  concat(timestampdiff(SECOND,r.trx_wait_started,CURRENT_TIMESTAMP()),'s') AS duration,\
	  b.trx_mysql_thread_id blocking_thread,t.processlist_command state,b.trx_query blocking_query,e.sql_text \
	  FROM information_schema.innodb_lock_waits w \
	  JOIN information_schema.innodb_trx b ON b.trx_id = w.blocking_trx_id \
	  JOIN information_schema.innodb_trx r ON r.trx_id = w.requesting_trx_id \
	  JOIN performance_schema.threads t on t.processlist_id = b.trx_mysql_thread_id \
	  JOIN performance_schema.events_statements_current e USING(thread_id) \G " >> ${logfile}
	          fi
	          sleep 5
	  done
	  
	  ```
	- log
	- ```
	  [root@VM-16-15-centos scripts]# cat innodb_lock_monitor.log
	  Thu Dec  8 19:35:09 CST 2022
	  *************************** 1. row ***************************
	   waiting_thread: 69026
	    waiting_query: delete from tmp where id < 10
	         duration: 4s
	  blocking_thread: 67882
	            state: Sleep
	   blocking_query: NULL
	         sql_text: delete from tmp where id = 1
	  Thu Dec  8 19:35:15 CST 2022
	  *************************** 1. row ***************************
	   waiting_thread: 69026
	    waiting_query: delete from tmp where id < 10
	         duration: 10s
	  blocking_thread: 67882
	            state: Sleep
	   blocking_query: NULL
	         sql_text: delete from tmp where id = 1
	  Thu Dec  8 19:35:20 CST 2022
	  
	  ```
- resources
	- [故障分析 | 有效解决 MySQL 行锁等待超时问题【建议收藏】 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/134053502)
	- [MySQL如何快速插入大量数据 - 风中琉璃 - 博客园 (cnblogs.com)](https://www.cnblogs.com/jiangzimin/p/10573168.html)
	-