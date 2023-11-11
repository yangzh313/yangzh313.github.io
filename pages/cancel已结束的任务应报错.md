- ## cancel 不存在的任务
- id:: 62cfe0bc-c533-4f33-abae-bc3c0e37d870
  ```
  [root@zymysql mysql-demo-1.0]# go run src/client/client.go cancel --task-id "8acdfe98-0327-4f7b-9c43-4cac1adf6b02"
  2022-07-14 17:16:22     INFO    cli/cancel.go:36        request info task_id:"8acdfe98-0327-4f7b-9c43-4cac1adf6b02"
  2022-07-14 17:16:22     ERROR   cli/cancel.go:39        client cancel failed; err: rpc error: code = Unknown desc = task id not found in agent record
  Error: rpc error: code = Unknown desc = task id not found in agent record
  2022-07-14 17:16:22     ERROR   logger/logger.go:103    Failed running "cancel"
  
  exit status 1
  ```
-
- ## cancel 非running任务
- ```
  [root@zymysql mysql-demo-1.0]# go run src/client/client.go cancel --task-id "8acdfe98-0327-4f7b-9c43-4cac1adf6b03"
  2022-07-14 17:15:39     INFO    cli/cancel.go:36        request info task_id:"8acdfe98-0327-4f7b-9c43-4cac1adf6b03"
  2022-07-14 17:15:39     ERROR   cli/cancel.go:39        client cancel failed; err: rpc error: code = Unknown desc = cannot cancel tasks that are not running
  Error: rpc error: code = Unknown desc = cannot cancel tasks that are not running
  2022-07-14 17:15:39     ERROR   logger/logger.go:103    Failed running "cancel"
  
  exit status 1
  ```