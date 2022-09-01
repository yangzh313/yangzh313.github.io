- client
- ```
  [root@zymysql mysql-demo-1.0]# go run src/client/client.go cancel --task-id "f71acba2-9c5c-49cd-995a-f85546bed95e"
  2022-07-14 17:37:36     INFO    cli/cancel.go:36        request info task_id:"f71acba2-9c5c-49cd-995a-f85546bed95e"
  2022-07-14 17:37:36     INFO    cli/cancel.go:42        request result task_id:"f71acba2-9c5c-49cd-995a-f85546bed95e" error_message:"ok" status:RUNNING
  
  ```
-
- server
- ```
  2022-07-14 17:37:29     INFO    mysqldb/backup.go:359   create FULL backup
  2022-07-14 17:37:29     INFO    mysqldb/backup.go:371   sh -c xtrabackup \
    --host 127.0.0.1 \
    --port 3306 \
    --user root \
    --password *** \
    --slave-info \
    --backup \
    --parallel 4 \
    --target-dir /mnt/backup/3306/1657791445/full \
  2022-07-14 17:37:36     INFO    src/agent.go:172        cancel task  start
  2022-07-14 17:37:36     INFO    src/agent.go:187        cancel task  sent
  2022-07-14 17:37:36     INFO    tasks/task.go:210       task stopped, response {"Code":6400,"Msg":"task failure by stop","Scope":"task","Fields":null}
  2022-07-14 17:37:36     INFO    tasks/taskImpl.go:73    recieved task stop
  2022-07-14 17:37:36     INFO    mysqldb/backup.go:114   recieved task stop
  2022-07-14 17:37:41     INFO    mysqldb/backup.go:374   out: xtrabackup: recognized server arguments: --log_bin=mysql-bin --server-id=1 --datadir=/var/lib/mysql --parallel=4
  xtrabackup: recognized client arguments: --host=127.0.0.1 --port=3306 --user=root --password=* --slave-info=1 --backup=1 --target-dir=/mnt/backup/3306/1657791445/full
  220714 17:37:29  version_check Connecting to MySQL server with DSN 'dbi:mysql:;mysql_read_default_group=xtrabackup;host=127.0.0.1;port=3306' as 'root'  (using password: YES).
  220714 17:37:29  version_check Connected to MySQL server
  220714 17:37:30  version_check Executing a version check against the server...
  220714 17:37:30  version_check Done.
  220714 17:37:30 Connecting to MySQL server host: 127.0.0.1, user: root, password: set, port: 3306, socket: not set
  Using server version 5.7.37-log
  xtrabackup version 2.4.26 based on MySQL server 5.7.35 Linux (x86_64) (revision id: 19de43b)
  xtrabackup: uses posix_fadvise().
  xtrabackup: cd to /var/lib/mysql
  xtrabackup: open files limit requested 0, set to 1024
  xtrabackup: using the following InnoDB configuration:
  xtrabackup:   innodb_data_home_dir = .
  xtrabackup:   innodb_data_file_path = ibdata1:12M:autoextend
  xtrabackup:   innodb_log_group_home_dir = ./
  xtrabackup:   innodb_log_files_in_group = 2
  xtrabackup:   innodb_log_file_size = 50331648
  InnoDB: Number of pools: 1
  220714 17:37:30 >> log scanned up to (2719118550)
  xtrabackup: Generating a list of tablespaces
  InnoDB: Allocated tablespace ID 2 for mysql/plugin, old maximum was 0
  xtrabackup: Starting 4 threads for parallel data files transfer
  220714 17:37:30 [03] Copying ./mysql/plugin.ibd to /mnt/backup/3306/1657791445/full/mysql/plugin.ibd
  220714 17:37:30 [02] Copying ./ibdata1 to /mnt/backup/3306/1657791445/full/ibdata1
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [01] Copying ./mysql/servers.ibd to /mnt/backup/3306/1657791445/full/mysql/servers.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [04] Copying ./mysql/help_topic.ibd to /mnt/backup/3306/1657791445/full/mysql/help_topic.ibd
  220714 17:37:30 [02]        ...done
  220714 17:37:30 [04]        ...done
  220714 17:37:30 [01] Copying ./mysql/help_relation.ibd to /mnt/backup/3306/1657791445/full/mysql/help_relation.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [03] Copying ./mysql/help_category.ibd to /mnt/backup/3306/1657791445/full/mysql/help_category.ibd
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [02] Copying ./mysql/help_keyword.ibd to /mnt/backup/3306/1657791445/full/mysql/help_keyword.ibd
  220714 17:37:30 [02]        ...done
  220714 17:37:30 [03] Copying ./mysql/time_zone_name.ibd to /mnt/backup/3306/1657791445/full/mysql/time_zone_name.ibd
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [01] Copying ./mysql/time_zone.ibd to /mnt/backup/3306/1657791445/full/mysql/time_zone.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [04] Copying ./mysql/time_zone_transition.ibd to /mnt/backup/3306/1657791445/full/mysql/time_zone_transition.ibd
  220714 17:37:30 [04]        ...done
  220714 17:37:30 [02] Copying ./mysql/time_zone_transition_type.ibd to /mnt/backup/3306/1657791445/full/mysql/time_zone_transition_type.ibd
  220714 17:37:30 [02]        ...done
  220714 17:37:30 [03] Copying ./mysql/time_zone_leap_second.ibd to /mnt/backup/3306/1657791445/full/mysql/time_zone_leap_second.ibd
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [01] Copying ./mysql/innodb_table_stats.ibd to /mnt/backup/3306/1657791445/full/mysql/innodb_table_stats.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [04] Copying ./mysql/innodb_index_stats.ibd to /mnt/backup/3306/1657791445/full/mysql/innodb_index_stats.ibd
  220714 17:37:30 [04]        ...done
  220714 17:37:30 [02] Copying ./mysql/slave_relay_log_info.ibd to /mnt/backup/3306/1657791445/full/mysql/slave_relay_log_info.ibd
  220714 17:37:30 [02]        ...done
  220714 17:37:30 [03] Copying ./mysql/slave_master_info.ibd to /mnt/backup/3306/1657791445/full/mysql/slave_master_info.ibd
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [01] Copying ./mysql/slave_worker_info.ibd to /mnt/backup/3306/1657791445/full/mysql/slave_worker_info.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [04] Copying ./mysql/gtid_executed.ibd to /mnt/backup/3306/1657791445/full/mysql/gtid_executed.ibd
  220714 17:37:30 [04]        ...done
  220714 17:37:30 [02] Copying ./mysql/server_cost.ibd to /mnt/backup/3306/1657791445/full/mysql/server_cost.ibd
  220714 17:37:30 [02]        ...done
  220714 17:37:30 [03] Copying ./mysql/engine_cost.ibd to /mnt/backup/3306/1657791445/full/mysql/engine_cost.ibd
  220714 17:37:30 [03]        ...done
  220714 17:37:30 [01] Copying ./sys/sys_config.ibd to /mnt/backup/3306/1657791445/full/sys/sys_config.ibd
  220714 17:37:30 [01]        ...done
  220714 17:37:30 [04] Copying ./test/sbtest10.ibd to /mnt/backup/3306/1657791445/full/test/sbtest10.ibd
  220714 17:37:30 [02] Copying ./test/sbtest8.ibd to /mnt/backup/3306/1657791445/full/test/sbtest8.ibd
  220714 17:37:30 [03] Copying ./test/sbtest3.ibd to /mnt/backup/3306/1657791445/full/test/sbtest3.ibd
  220714 17:37:30 [01] Copying ./test/sbtest5.ibd to /mnt/backup/3306/1657791445/full/test/sbtest5.ibd
  220714 17:37:31 >> log scanned up to (2719118550)
  220714 17:37:31 [04]        ...done
  220714 17:37:31 [01]        ...done
  220714 17:37:31 [02]        ...done
  220714 17:37:31 [03]        ...done
  220714 17:37:32 >> log scanned up to (2719118550)
  220714 17:37:33 >> log scanned up to (2719118550)
  220714 17:37:34 >> log scanned up to (2719118550)
  220714 17:37:35 >> log scanned up to (2719118550)
  220714 17:37:36 >> log scanned up to (2719118550)
  , err: signal: killed
  2022-07-14 17:37:41     INFO    common/utils.go:144     start exec umount /mnt/backup/3306/1657791445
  2022-07-14 17:37:44     INFO    storage/api.go:281      clear FULL backup
  2022-07-14 17:37:44     INFO    logger/logger.go:87     share id 51 deleted
  2022-07-14 17:37:44     INFO    logger/logger.go:87     Delete dataset zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1657791445
  2022-07-14 17:37:45     INFO    storage/api.go:143      dataset zpool/site-061858a0-3b41-4e39-87cb-88bf6301bbed/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1657791445 deleted
  2022-07-14 17:37:45     INFO    tasks/task.go:125       task update
  2022-07-14 17:37:45     INFO    tasks/task.go:138       task output: &{TaskId:f71acba2-9c5c-49cd-995a-f85546bed95e Done:false Status:code:6408, msg:received task stop Progress:0.2 Size:0 EndTime:}
  
  ```