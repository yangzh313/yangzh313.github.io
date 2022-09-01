- client
- ```
  [root@zymysql mysql-demo-1.0]# go run src/client/client.go query --task-id "8e311d42-7f88-4f36-bc17-c26b9cf27d81"
  2022-07-14 20:07:15     INFO    cli/query.go:36 request info task_id:"8e311d42-7f88-4f36-bc17-c26b9cf27d81"
  2022-07-14 20:07:15     INFO    cli/query.go:42 request result task_id:"8e311d42-7f88-4f36-bc17-c26b9cf27d81" error_code:6400 error_message:"task failure by stop" status:FAILURE process:0.2
  ```
-
- server
- ```
  2022-07-14 20:07:15     INFO    src/agent.go:150        qr: {TaskId:8e311d42-7f88-4f36-bc17-c26b9cf27d81 Done:false Status:code:6400, msg:task failure by stop Progress:0.2 Size:0 EndTime:}
  2022-07-14 20:07:15     INFO    src/agent.go:158        query task end: task_id:"8e311d42-7f88-4f36-bc17-c26b9cf27d81"  error_code:6400  error_message:"task failure by stop"  status:FAILURE  process:0.2
  ```