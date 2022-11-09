# demo

[Q&A](https://www.notion.so/Q-A-c23032e71af0496282010ed16c1d2179)

## Step

```bash
展示实例数据信息
发起全量备份后，发起首次日志备份。
发起增量备份，插入一些数据后，发起二次日志备份。
用增量备份点，验证恢复/挂载恢复 功能。
演示卸载挂载恢复的实例
```

## 环境

集群：10.140.28.10

虚拟机：10.142.192.211

MySQL：版本5.7.37

Nas:  [10.142.192.38](http://10.142.192.38/)

## discover & dbdiscover

```bash
[root@zymysql ~]# ar-dbclient discover
2022-10-25 17:10:54     INFO    cmd/discover.go:67      request info task_id:"5cc49bd3-31c2-46b1-8c19-873c6ef8960b" resource_type:1 resource_ip:"127.0.0.1"
2022-10-25 17:10:54     INFO    cmd/discover.go:73      request result task_id:"5cc49bd3-31c2-46b1-8c19-873c6ef8960b" error_message:"ok" db_info:{resource_type:1 resource_version:"5.7.37" port:3306}

[root@zymysql ~]# ar-dbclient discover --port 3306
2022-10-25 17:11:22     INFO    cmd/discover.go:54      request info task_id:"10654453-2a32-4fe1-aa25-c386f984a9fe" resource_ip:"127.0.0.1" resource_type:1 user_name:"root" user_password:"C+GbNRpO0dk+/FOBAKSO6A==" instance_port:3306
2022-10-25 17:11:22     INFO    cmd/discover.go:60      request result task_id:"10654453-2a32-4fe1-aa25-c386f984a9fe" error_message:"ok" db_info:{user_name:"root" user_password:"C+GbNRpO0dk+/FOBAKSO6A==" data_path:"/var/lib/mysql" arch_mode:"1" arch_path:"/var/lib/mysql" instance_port:3306}
```

### full backup

创建全量备份

```bash
~ # protector-client-py create-mysql-backup --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --management-platform CS --source-type MySQL --resource-meta '{ "database": { "agent_id": "3d475dd0-6d60-4744-86d8-e78df2a8c789", "agent_addr": "10.142.192.211:3421", "resource_ip": "127.0.0.1", "resource_type": "mysql", "resource": [ { "resource_port": 3306, "resource_version": "5.7.37", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "data_path": "/var/lib/mysql", "arch_mode": 1, "arch_path": "/var/lib/mysql" } ] }, "nas": { "ip": "10.142.192.38", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "url": "http://10.142.192.38/api/v2.0", "pool": "zpool", "compression": true, "timeout": 5 } }' --backup-type full
~ # protector-client-py show-dbbackup --backup-id "c34e0891-89cd-4f22-b74a-a15d8274cb11"
{
        "status":       0,
        "data": {
                "backup_id":    "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "name": "1666947871",
                "timestamp":    "",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                "object_type":  "MySQL",
                "type": "full backup",
                "state":        "available",
                "size": 1334807618,
                "task_id":      "b6fa7e9b-a45d-4d47-a2f2-429c2963683f",
                "compression":  "",
                "parent_id":    "",
                "snapshot_path":        "zpool/site-7ccedfad-b9a3-4461-a49d-e448b51ba785/09d6416a-f90a-11ec-97cf-60da833d46f7/3d475dd0-6d60-4744-86d8-e78df2a8c789/3306/1666949546947@c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "source_type":  "MySQL",
                "management_platform":  "CS",
                "description":  "create DBBackup:  to 7ccedfad-b9a3-4461-a49d-e448b51ba785 on host: archcnstcm4379",
                "arch": "",
                "agent_addr":   "10.142.192.211:3421",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "port": 3306,
                "data_type":    "data",
                "resource_version":     "5.7.37",
                "end_time":     "2022-10-28 17:04:48",
                "relation_backup_id":   "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "resource":     {
                        "database":     {
                                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                                "agent_addr":   "10.142.192.211:3421",
                                "resource_ip":  "127.0.0.1",
                                "resource_type":        "mysql",
                                "resource":     [{
                                                "resource_port":        3306,
                                                "resource_version":     "5.7.37",
                                                "user": "root",
                                                "data_path":    "/var/lib/mysql",
                                                "arch_mode":    1,
                                                "arch_path":    "/var/lib/mysql"
                                        }]
                        },
                        "nas":  {
                                "ip":   "10.142.192.38",
                                "user": "root",
                                "url":  "http://10.142.192.38/api/v2.0",
                                "pool": "zpool",
                                "compression":  true,
                                "timeout":      5
                        }
                }
        }
}
```

### logbackup1

全量日志备份

```bash
~ # protector-client-py create-dblog-backup --source-type DB_LOG --end-time "2022-10-28 17:04:48" --relation-backup-id "c34e0891-89cd-4f22-b74a-a15d8274cb11" --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --management-platform CS --resource-meta '{ "database": { "agent_id": "3d475dd0-6d60-4744-86d8-e78df2a8c789", "agent_addr": "10.142.192.211:3421", "resource_ip": "127.0.0.1", "resource_type": "mysql", "resource": [ { "resource_port": 3306, "resource_version": "5.7.37", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "data_path": "/var/lib/mysql", "arch_mode": 1, "arch_path": "/var/lib/mysql" } ] }, "nas": { "ip": "10.142.192.38", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "url": "http://10.142.192.38/api/v2.0", "pool": "zpool", "compression": true, "timeout": 5 } }'

~ # protector-client-py show-dbbackup --backup-id "e667d910-e78d-4f04-a518-b3922eb71f19"
{
        "status":       0,
        "data": {
                "backup_id":    "e667d910-e78d-4f04-a518-b3922eb71f19",
                "name": "1666948015",
                "timestamp":    "1666948020",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                "object_type":  "MySQL",
                "type": "full backup",
                "state":        "available",
                "size": 619,
                "task_id":      "ec04a4a2-af12-4eb2-809c-e5060f9e1a2f",
                "compression":  "",
                "parent_id":    "",
                "snapshot_path":        "",
                "source_type":  "DB_LOG",
                "management_platform":  "CS",
                "description":  "",
                "arch": "",
                "agent_addr":   "10.142.192.211:3421",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "port": 3306,
                "data_type":    "log",
                "resource_version":     "5.7.37",
                "end_time":     "1666948016",
                "relation_backup_id":   "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "resource":     {
                        "database":     {
                                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                                "agent_addr":   "10.142.192.211:3421",
                                "resource_ip":  "127.0.0.1",
                                "resource_type":        "mysql",
                                "resource":     [{
                                                "resource_port":        3306,
                                                "resource_version":     "5.7.37",
                                                "user": "root",
                                                "data_path":    "/var/lib/mysql",
                                                "arch_mode":    1,
                                                "arch_path":    "/var/lib/mysql"
                                        }]
                        },
                        "nas":  {
                                "ip":   "10.142.192.38",
                                "user": "root",
                                "url":  "http://10.142.192.38/api/v2.0",
                                "pool": "zpool",
                                "compression":  true,
                                "timeout":      5
                        },
                        "log-backup-id":        "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                        "end-time":     "2022-10-28 17:04:48"
                }
        }
}
```

### incbackup

增量备份

```bash
mysql> truncate t1;
Query OK, 0 rows affected (0.10 sec)
mysql> insert into t1 values(null, now());
Query OK, 1 row affected (0.01 sec)

mysql> select * from t1;
+----+---------------------+
| id | addtime             |
+----+---------------------+
|  1 | 2022-10-28 17:08:30 |
+----+---------------------+
1 row in set (0.00 sec)

~ # protector-client-py create-mysql-backup --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --management-platform CS --source-type MySQL --resource-meta '{ "database": { "agent_id": "3d475dd0-6d60-4744-86d8-e78df2a8c789", "agent_addr": "10.142.192.211:3421", "resource_ip": "127.0.0.1", "resource_type": "mysql", "resource": [ { "resource_port": 3306, "resource_version": "5.7.37", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "data_path": "/var/lib/mysql", "arch_mode": 1, "arch_path": "/var/lib/mysql" } ] }, "nas": { "ip": "10.142.192.38", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "url": "http://10.142.192.38/api/v2.0", "pool": "zpool", "compression": true, "timeout": 5 } }'

~ # protector-client-py show-dbbackup --backup-id "7be1f05e-b855-40ce-872e-ede45397cb25"
{
        "status":       0,
        "data": {
                "backup_id":    "7be1f05e-b855-40ce-872e-ede45397cb25",
                "name": "1666948144",
                "timestamp":    "",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                "object_type":  "MySQL",
                "type": "incremental backup",
                "state":        "available",
                "size": 11456287,
                "task_id":      "a4d4c5a8-e8fb-4be5-afe9-ddafc11e881c",
                "compression":  "",
                "parent_id":    "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "snapshot_path":        "zpool/site-7ccedfad-b9a3-4461-a49d-e448b51ba785/09d6416a-f90a-11ec-97cf-60da833d46f7/3d475dd0-6d60-4744-86d8-e78df2a8c789/3306/1666949546947@7be1f05e-b855-40ce-872e-ede45397cb25",
                "source_type":  "MySQL",
                "management_platform":  "CS",
                "description":  "create DBBackup:  to 7ccedfad-b9a3-4461-a49d-e448b51ba785 on host: archcnstcm4379",
                "arch": "",
                "agent_addr":   "10.142.192.211:3421",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "port": 3306,
                "data_type":    "data",
                "resource_version":     "5.7.37",
                "end_time":     "2022-10-28 17:09:11",
                "relation_backup_id":   "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "resource":     {
                        "database":     {
                                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                                "agent_addr":   "10.142.192.211:3421",
                                "resource_ip":  "127.0.0.1",
                                "resource_type":        "mysql",
                                "resource":     [{
                                                "resource_port":        3306,
                                                "resource_version":     "5.7.37",
                                                "user": "root",
                                                "data_path":    "/var/lib/mysql",
                                                "arch_mode":    1,
                                                "arch_path":    "/var/lib/mysql"
                                        }]
                        },
                        "nas":  {
                                "ip":   "10.142.192.38",
                                "user": "root",
                                "url":  "http://10.142.192.38/api/v2.0",
                                "pool": "zpool",
                                "compression":  true,
                                "timeout":      5
                        }
                }
        }
}
```

### logbackup2

第二次日志备份

```bash
mysql> insert into t1 values(null, now());
Query OK, 1 row affected (0.05 sec)

mysql> select * from t1;
+----+---------------------+
| id | addtime             |
+----+---------------------+
|  1 | 2022-10-28 17:08:30 |
|  2 | 2022-10-28 17:10:22 |
+----+---------------------+
2 rows in set (0.00 sec)

~ # protector-client-py create-dblog-backup --source-type DB_LOG --end-time "2022-10-28 14:55:49" --relation-backup-id "04033667-8840-4b69-86f9-854eb5668649" --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --management-platform CS --resource-meta '{ "database": { "agent_id": "3d475dd0-6d60-4744-86d8-e78df2a8c789", "agent_addr": "10.142.192.211:3421", "resource_ip": "127.0.0.1", "resource_type": "mysql", "resource": [ { "resource_port": 3306, "resource_version": "5.7.37", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "data_path": "/var/lib/mysql", "arch_mode": 1, "arch_path": "/var/lib/mysql" } ] }, "nas": { "ip": "10.142.192.38", "user": "root", "password": "C+GbNRpO0dk+/FOBAKSO6A==", "url": "http://10.142.192.38/api/v2.0", "pool": "zpool", "compression": true, "timeout": 5 } }'

~ # protector-client-py show-dbbackup --backup-id "a9ea9361-619f-41be-958b-303b502715a0"
{
        "status":       0,
        "data": {
                "backup_id":    "a9ea9361-619f-41be-958b-303b502715a0",
                "name": "1666948258",
                "timestamp":    "1666948260",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                "object_type":  "MySQL",
                "type": "incremental backup",
                "state":        "available",
                "size": 1452,
                "task_id":      "563f7cb7-8a25-49a6-ad26-6db6c5c3ecdb",
                "compression":  "",
                "parent_id":    "e667d910-e78d-4f04-a518-b3922eb71f19",
                "snapshot_path":        "",
                "source_type":  "DB_LOG",
                "management_platform":  "CS",
                "description":  "",
                "arch": "",
                "agent_addr":   "10.142.192.211:3421",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "port": 3306,
                "data_type":    "log",
                "resource_version":     "5.7.37",
                "end_time":     "1666948256",
                "relation_backup_id":   "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                "resource":     {
                        "database":     {
                                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                                "agent_addr":   "10.142.192.211:3421",
                                "resource_ip":  "127.0.0.1",
                                "resource_type":        "mysql",
                                "resource":     [{
                                                "resource_port":        3306,
                                                "resource_version":     "5.7.37",
                                                "user": "root",
                                                "data_path":    "/var/lib/mysql",
                                                "arch_mode":    1,
                                                "arch_path":    "/var/lib/mysql"
                                        }]
                        },
                        "nas":  {
                                "ip":   "10.142.192.38",
                                "user": "root",
                                "url":  "http://10.142.192.38/api/v2.0",
                                "pool": "zpool",
                                "compression":  true,
                                "timeout":      5
                        },
                        "log-backup-id":        "c34e0891-89cd-4f22-b74a-a15d8274cb11",
                        "end-time":     "2022-10-28 17:04:48"
                }
        }
}
```

### restore

物理恢复，停止实例，删除数据目录，使用增量进行恢复并恢复到时间点

```bash
[root@zymysql mysql-demo-1.0]# systemctl stop mysqld
[root@zymysql mysql-demo-1.0]# systemctl status mysqld
[root@zymysql ~]# mv /var/lib/mysql /var/lib/mysql.bak
[root@archcnstcm4379 /]# protector-client-py restore-mysql-backup --backup-id "7be1f05e-b855-40ce-872e-ede45397cb25" --target-time "1666948256" --resource-type MySQL --target-port 3306 --agent-id "3d475dd0-6d60-4744-86d8-e78df2a8c789" --agent-addr 10.142.192.211:3421 --restore-path /var/lib/mysql --cmdrun "systemctl start mysqld" --cover true --pre-script "/root/zy/scripts/pre_script.sh" --post-script "/root/zy/scripts/post_script.sh"

~ # protector-client-py show-task --task-id "7dc6351a-e6d0-461d-8d1f-27d700e59219"
{
        "status":       0,
        "data": {
                "name": "1666691545",
                "task_id":      "7dc6351a-e6d0-461d-8d1f-27d700e59219",
                "type": "restore-mysql-backup",
                "backup_id":    "362c4a27-b0c5-4e6e-90d2-5d01074a72e8",
                "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "start_time":   "2022-Oct-25 17:52:25",
                "end_time":     "2022-Oct-25 17:53:02",
                "hostname":     "archcnstcm4379",
                "server_ip":    "10.10.11.14",
                "source_type":  "MySQL",
                "management_platform":  "CS",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "leader_task_id":       "7dc6351a-e6d0-461d-8d1f-27d700e59219",
                "status":       "complete",
                "process":      1.000000,
                "description":  "restore dbbackup: 362c4a27-b0c5-4e6e-90d2-5d01074a72e8 from snapshot on host: archcnstcm4379",
                "error_code":   "",
                "error_message":        "",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "resource_port":        3306,
                "entity_type":  "db"
        }
}

[root@zymysql mysql-demo-1.0]# systemctl status mysqld
[root@zymysql ~]# ll /var/lib/mysql
mysql> select @@port;
+--------+
| @@port |
+--------+
|   3306 |
+--------+
1 row in set (0.00 sec)

mysql>
mysql> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql>
mysql> select * from t1;
+----+---------------------+
| id | addtime             |
+----+---------------------+
|  1 | 2022-10-27 13:52:14 |
|  2 | 2022-10-27 13:55:09 |
+----+---------------------+
2 rows in set (0.00 sec)
```

### umount

实例卸载

```bash
[root@archcnstcm4379 /]# protector-client-py db-umount --agent-id "3d475dd0-6d60-4744-86d8-e78df2a8c789" --resource-port 3306 --source-type MySQL --management-platform CS --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --keep-restore-path yes
{
        "status":       0,
        "data": {
                "task_id":      "d6ba266a-b0ed-4ab8-86ea-49c532ae0650"
        }
}
[root@archcnstcm4379 /]#
[root@archcnstcm4379 /]#
[root@archcnstcm4379 /]# protector-client-py show-task --task-id "d6ba266a-b0ed-4ab8-86ea-49c532ae0650"
{
        "status":       0,
        "data": {
                "name": "1665303100",
                "task_id":      "d6ba266a-b0ed-4ab8-86ea-49c532ae0650",
                "type": "db-umount",
                "backup_id":    "b16643f2-a90f-4246-93cd-fc49fc1b529f",
                "object_id":    "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                "start_time":   "2022-Oct-09 16:11:40",
                "end_time":     "2022-Oct-09 08:11:45",
                "hostname":     "archcnstcm4379",
                "server_ip":    "10.10.11.14",
                "source_type":  "MySql",
                "management_platform":  "CS",
                "site_id":      "",
                "leader_task_id":       "d6ba266a-b0ed-4ab8-86ea-49c532ae0650",
                "status":       "complete",
                "process":      1.000000,
                "description":  "db umount: b16643f2-a90f-4246-93cd-fc49fc1b529f",
                "error_code":   "",
                "error_message":        "ok",
                "agent_id":     "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                "resource_port":        3320,
                "entity_type":  "db"
        }
}

[root@zymysql ~]# systemctl status mysqld
[root@zymysql ~]# systemctl start mysqld
[root@zymysql ~]# systemctl status mysqld
```

### mount

在nas上快速拉起实例并恢复到时间点

```bash
~ # protector-client-py mysql-mount --backup-id "7be1f05e-b855-40ce-872e-ede45397cb25" --target-time "1666948256" --resource-type MySQL --target-port 3323 --agent-id "3d475dd0-6d60-4744-86d8-e78df2a8c789" --agent-addr 10.142.192.211:3421 --pre-script "/root/zy/scripts/pre_script.sh" --post-script "/root/zy/scripts/post_script.sh"

~ # protector-client-py show-task --task-id "c08eb0d9-4fb3-474a-80d2-d835ae599b83"
{
        "status":       0,
        "data": {
                "name": "1666691922",
                "task_id":      "c08eb0d9-4fb3-474a-80d2-d835ae599b83",
                "type": "restore-mysql-backup",
                "backup_id":    "362c4a27-b0c5-4e6e-90d2-5d01074a72e8",
                "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "start_time":   "2022-Oct-25 17:58:42",
                "end_time":     "2022-Oct-25 17:59:07",
                "hostname":     "archcnstcm4379",
                "server_ip":    "10.10.11.14",
                "source_type":  "MySQL",
                "management_platform":  "CS",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "leader_task_id":       "c08eb0d9-4fb3-474a-80d2-d835ae599b83",
                "status":       "complete",
                "process":      1.000000,
                "description":  "restore dbbackup: 362c4a27-b0c5-4e6e-90d2-5d01074a72e8 from snapshot on host: archcnstcm4379",
                "error_code":   "",
                "error_message":        "",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "resource_port":        3322,
                "entity_type":  "db"
        }
}

[root@zymysql ~]# netstat -ntlp|grep 3323
tcp6       0      0 :::3322                 :::*                    LISTEN      113924/mysqld

[root@zymysql ~]# mysql -uroot -p -h127.0.0.1 -P3323
mysql> select @@port;
+--------+
| @@port |
+--------+
|   3322 |
+--------+
1 row in set (0.00 sec)
mysql> use test;
mysql> select * from t1;
+----+---------------------+
| id | addtime             |
+----+---------------------+
|  1 | 2022-10-27 13:52:14 |
|  2 | 2022-10-27 13:55:09 |
+----+---------------------+
2 rows in set (0.01 sec)
```

### dbumount

实例卸载

```bash
[root@archcnstcm4379 /]# protector-client-py db-umount --agent-id "3d475dd0-6d60-4744-86d8-e78df2a8c789" --resource-port 3323 --source-type MySQL --management-platform CS --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785"                                                   {
        "status":       0,
        "data": {
                "task_id":      "65cf7602-bbc9-407f-8bf9-d5b7bd27bd27"
        }
}
~ # protector-client-py show-task --task-id "6dfe9d85-4aa6-4e54-98f0-b60d9349021d"
{
        "status":       0,
        "data": {
                "name": "1666692101",
                "task_id":      "6dfe9d85-4aa6-4e54-98f0-b60d9349021d",
                "type": "db-umount",
                "backup_id":    "362c4a27-b0c5-4e6e-90d2-5d01074a72e8",
                "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "start_time":   "2022-Oct-25 18:01:41",
                "end_time":     "2022-Oct-25 18:01:55",
                "hostname":     "archcnstcm4379",
                "server_ip":    "10.10.11.14",
                "source_type":  "MySql",
                "management_platform":  "CS",
                "site_id":      "",
                "leader_task_id":       "6dfe9d85-4aa6-4e54-98f0-b60d9349021d",
                "status":       "complete",
                "process":      1.000000,
                "description":  "db umount: 362c4a27-b0c5-4e6e-90d2-5d01074a72e8",
                "error_code":   "",
                "error_message":        "ok",
                "agent_id":     "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                "resource_port":        3322,
                "entity_type":  "db"
        }
}

[root@zymysql ~]# netstat -ntlp|grep 3323
```

### delete backup

```bash
[root@archcnstcm4379 /]# protector-client-py delete-mysql-backup --backup-id "aaed7ee9-8f16-4bb4-85d8-eb779a16ed14"
{
        "status":       0,
        "data": {
                "task_id":      "49182c46-6252-4ede-8d2e-c4bb08dde9bd"
        }
}
[root@archcnstcm4379 /]#
[root@archcnstcm4379 /]# protector-client-py show-dbbackup --backup-id "49182c46-6252-4ede-8d2e-c4bb08dde9bd"
{"status":2,"error_code":"ArP1007E_db_query_error","error_message":"ArP1007E Database query operation failed:object not persistent backup id: 49182c46-6252-4ede-8d2e-c4bb08dde9bd not exists."}
[root@archcnstcm4379 /]#
[root@archcnstcm4379 /]#
[root@archcnstcm4379 /]# protector-client-py show-dbbackup --backup-id "b16643f2-a90f-4246-93cd-fc49fc1b529f"
{
        "status":       0,
        "data": {
                "backup_id":    "b16643f2-a90f-4246-93cd-fc49fc1b529f",
                "name": "1665293771",
                "timestamp":    "",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "cluster_id":   "09d6416a-f90a-11ec-97cf-60da833d46f7",
                "object_type":  "instance",
                "type": "incremental backup",
                "state":        "available",
                "size": 2488503,
                "task_id":      "42426f7d-04ab-4931-a9ab-0c4c1142a08f",
                "compression":  "",
                "parent_id":    "",
                "snapshot_path":        "",
                "source_type":  "MySQL",
                "management_platform":  "CS",
                "description":  "create DBBackup:  to 7ccedfad-b9a3-4461-a49d-e448b51ba785 on host: archcnstcm4379",
                "arch": "",
                "agent_addr":   "10.142.192.211:3421",
                "agent_id":     "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                "port": 3306,
                "data_type":    "data",
                "resource_version":     "5.7.37",
                "end_time":     "2022-10-09 13:36:18",
                "relation_backup_id":   "",
                "resource":     {
                        "database":     {
                                "agent_id":     "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                                "agent_addr":   "10.142.192.211:3421",
                                "resource_ip":  "127.0.0.1",
                                "resource_type":        "mysql",
                                "resource":     [{
                                                "resource_port":        3306,
                                                "resource_version":     "5.7.37",
                                                "user": "root",
                                                "data_path":    "/var/lib/mysql",
                                                "arch_mode":    1,
                                                "arch_path":    "/var/lib/mysql"
                                        }]
                        },
                        "nas":  {
                                "ip":   "10.142.192.38",
                                "user": "root",
                                "url":  "http://10.142.192.38/api/v2.0",
                                "pool": "zpool",
                                "compression":  true,
                                "timeout":      5
                        }
                }
        }
}
```

### expire

```bash
[root@archcnstcm4380 /]# protector-client-py expire-db-log --agent-id "3d475dd0-6d60-4744-86d8-e78df2a8c789" --resource-port 3306 --site-id "7ccedfad-b9a3-4461-a49d-e448b51ba785" --expire-time "1666941116"  --resource-type MySQL --agent-addr 10.142.192.211:3421 --management-platform CS --cluster-id "09d6416a-f90a-11ec-97cf-60da833d46f7"
{
        "status":       0,
        "data": {
                "task_id":      "d0cea726-7f45-4696-9b13-028b408448f3"
        }
}
[root@archcnstcm4380 /]# protector-client-py show-task --task-id "d0cea726-7f45-4696-9b13-028b408448f3"
{
        "status":       0,
        "data": {
                "name": "1665394672",
                "task_id":      "d0cea726-7f45-4696-9b13-028b408448f3",
                "type": "expire-db-log",
                "backup_id":    "expire db log without backup_id",
                "object_id":    "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                "start_time":   "2022-Oct-10 17:37:52",
                "end_time":     "2022-Oct-10 17:37:57",
                "hostname":     "archcnstcm4380",
                "server_ip":    "10.10.11.11",
                "source_type":  "DB_LOG",
                "management_platform":  "CS",
                "site_id":      "7ccedfad-b9a3-4461-a49d-e448b51ba785",
                "leader_task_id":       "d0cea726-7f45-4696-9b13-028b408448f3",
                "status":       "complete",
                "process":      1.000000,
                "description":  "Expire Log: site_id: 7ccedfad-b9a3-4461-a49d-e448b51ba785 on host: archcnstcm4380",
                "error_code":   "",
                "error_message":        "",
                "agent_id":     "0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3",
                "resource_port":        3306,
                "entity_type":  "db"
        }
}
```