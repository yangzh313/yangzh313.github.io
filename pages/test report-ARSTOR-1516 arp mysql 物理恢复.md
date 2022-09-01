- ## [[Aug 30th, 2022]]
- ### 发起恢复到时间点
- ```
  [root@zymysql mysql-demo-1.0]# go run src/cli/main.go restore --port 3308 --target-time 1660635142 --snapshot zpool/site-702236c5-5af8-4550-8215-2d23712e1590/09d6416a-f90a-11ec-97cf-60da833d46f7/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1660714729713@a1c38732-d475-410f-b515-d1e113d7b4c9  --backup-id a1c38732-d475-410f-b515-d1e113d7b4c9 --path /mysql/mydata/3308
  2022-08-30 14:50:20     INFO    cmd/restore.go:88       request info task_id:"8b45c406-e8bb-4b4b-aa75-6152bb22af26" resource_ip:"127.0.0.1" resource_type:1 user_name:"root" user_password:"C+GbNRpO0dk+/FOBAKSO6A==" target_port:3308 restore_path:"/mysql/mydata/3308" target_time:"1660635142" backup_id:"a1c38732-d475-410f-b515-d1e113d7b4c9" snapshot_path:"zpool/site-702236c5-5af8-4550-8215-2d23712e1590/09d6416a-f90a-11ec-97cf-60da833d46f7/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1660714729713@a1c38732-d475-410f-b515-d1e113d7b4c9" nas:{nas_ip:"10.142.192.69" user:"root" password:"C+GbNRpO0dk+/FOBAKSO6A==" url:"http://10.142.192.69/api/v2.0" pool:"zpool" compression:true timeout:5} pre_script:"/opt/ardbagent/script/prescript.sh" post_script:"/opt/ardbagent/script/postscript.sh" mount_id:"9df680bb-26c3-4d92-a419-59d49fa2f698"
  2022-08-30 14:50:20     INFO    cmd/restore.go:94       request result task_id:"8b45c406-e8bb-4b4b-aa75-6152bb22af26" error_message:"ok"
  ```
- ### 查询任务状态
- ```
  [root@zymysql mysql-demo-1.0]# go run src/cli/main.go query-restore --task-id 8b45c406-e8bb-4b4b-aa75-6152bb22af26
  2022-08-30 14:53:25     INFO    cmd/query_restore.go:30 request info task_id:"8b45c406-e8bb-4b4b-aa75-6152bb22af26"
  2022-08-30 14:53:25     INFO    cmd/query_restore.go:36 request result task_id:"8b45c406-e8bb-4b4b-aa75-6152bb22af26" error_message:"ok" status:SUCCESS progress:1
  ```
- ### 实例情况
- ```
  [root@zymysql mydata]# netstat -ntlp|grep 3308
  tcp6       0      0 :::3308                 :::*                    LISTEN      114725/mysqld
  [root@zymysql mydata]#
  [root@zymysql mydata]# ps -ef|grep 114725
  mysql    114725      1  0 14:50 pts/0    00:00:03 /usr/sbin/mysqld --defaults-file=/opt/ardbagent/etc/mysql/my.cnf --datadir=/mysql/mydata/3308 --user=mysql --socket=/mysql/mydata/3308/mysql.sock --log-error=/mysql/mydata/3308/mysql.log --pid-file=/mysql/mydata/3308/mysql.pid --port=3308
  
  ```