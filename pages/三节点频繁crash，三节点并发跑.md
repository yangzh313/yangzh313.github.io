public:: false

- 三节点create backup
- ```
  ~/zy # sh backup_zl.sh
  {
          "status":       0,
          "data": {
                  "task_id":      "a9943133-46a3-4816-bc3c-d0673c892750"
          }
  }
  
  ~/zy # sh backup.sh
  {
          "status":       0,
          "data": {
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791"
          }
  }
  
  ~/zy # sh backup_mj.sh
  {
          "status":       0,
          "data": {
                  "task_id":      "63cea210-0592-4993-8a14-4b26eb84cca7"
          }
  }
  
  ```
- 11 kill pserv
- ```
  [root@archcnstcm4379 bin]# kill 53260
  
  ```
- 12 resume task
- ```
  ~/zy # protector-client-py show-task --task-id "a9943133-46a3-4816-bc3c-d0673c892750"
  {
          "status":       0,
          "data": {
                  "name": "1661858566",
                  "task_id":      "a9943133-46a3-4816-bc3c-d0673c892750",
                  "type": "create-nas-backup",
                  "backup_id":    "b6f93316-9e06-42b6-be83-889ea81177bc",
                  "object_id":    "ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e",
                  "start_time":   "2022-Aug-30 19:22:46",
                  "end_time":     "",
                  "hostname":     "archcnstcm4379",
                  "server_ip":    "10.10.11.14",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "a9943133-46a3-4816-bc3c-d0673c892750",
                  "status":       "pause",
                  "process":      0.790000,
                  "description":  "create backup: ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4379",
                  "error_code":   "ArP1064E_trans_task_failure_by_lease_failed",
                  "error_message":        "task failure by lease failed ",
                  "sub_tasks":    [{
                                  "object_id":    "c776616a-f99b-4688-bf6d-f3d58ace782c",
                                  "start_time":   "2022-Aug-30 19:22:46",
                                  "end_time":     "",
                                  "hostname":     "archcnstcm4379",
                                  "server_ip":    "10.10.11.14",
                                  "status":       "complete",
                                  "task_id":      "f4b5829e-031b-4fb1-ad4e-0e6ecf8b7703",
                                  "description":  "create backup: ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4379",
                                  "process":      1.000000
                          }]
          }
  }
  ~/zy # protector-client-py resume-task --task-id "a9943133-46a3-4816-bc3c-d0673c892750"
  {
          "status":       0,
          "data": {
                  "task_id":      "a9943133-46a3-4816-bc3c-d0673c892750",
                  "type": "create-nas-backup"
          }
  }
  
  ```
- 12 kill pserv
- ```
  [root@archcnstcm4380 ~]# docker stop pserv1.4
  [root@archcnstcm4380 ~]# docker start pserv1.4
  pserv1.4
  ```
- 13 resume task
- ```
  ~/zy # protector-client-py show-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "name": "1661858589",
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup",
                  "backup_id":    "2e36b55f-8204-470d-84d1-eb543458f891",
                  "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                  "start_time":   "2022-Aug-30 19:23:09",
                  "end_time":     "2022-Aug-30 19:27:08",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "706bf418-8c2b-473a-a234-46af893cd791",
                  "status":       "pause",
                  "process":      0.540000,
                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                  "error_code":   "ArP1114E_task_timeout_caused_by_node_down",
                  "error_message":        "ArP1114E task timeout as the node running the task was down.",
                  "sub_tasks":    [{
                                  "object_id":    "8034d669-cb71-4f79-b453-9adbc308e7ff",
                                  "start_time":   "2022-Aug-30 19:23:09",
                                  "end_time":     "2022-Aug-30 19:27:08",
                                  "hostname":     "archcnstcm4380",
                                  "server_ip":    "10.10.11.11",
                                  "status":       "pause",
                                  "task_id":      "2f3e31d8-1bf5-40f3-ae80-b6fe5f4f1f10",
                                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                                  "process":      0.000000
                          }]
          }
  }
  ~/zy #
  ~/zy #
  ~/zy # protector-client-py resume-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup"
          }
  }
  
  ```
- 13 kill pserv
- ```
  [root@archcnstcm4381 ~]# docker restart pserv1.4
  pserv1.4
  ```
- 11 resume task
- ```
  ~/zy # protector-client-py show-task --task-id "f657004c-5d74-427a-9e2b-2ced15d26d61"
  {
          "status":       0,
          "data": {
                  "name": "1661858908",
                  "task_id":      "f657004c-5d74-427a-9e2b-2ced15d26d61",
                  "type": "create-nas-backup",
                  "backup_id":    "ff380431-b78c-4340-9c58-956bf4b9528a",
                  "object_id":    "97ce1470-b190-485a-8a3c-842d51756ed3",
                  "start_time":   "2022-Aug-30 19:28:28",
                  "end_time":     "2022-Aug-30 19:29:55",
                  "hostname":     "archcnstcm4381",
                  "server_ip":    "10.10.11.17",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "f657004c-5d74-427a-9e2b-2ced15d26d61",
                  "status":       "pause",
                  "process":      0.240000,
                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                  "error_code":   "ArP1114E_task_timeout_caused_by_node_down",
                  "error_message":        "ArP1114E task timeout as the node running the task was down.",
                  "sub_tasks":    [{
                                  "object_id":    "7fb00509-c74e-4872-8bc2-e3047fa9a230",
                                  "start_time":   "2022-Aug-30 19:28:28",
                                  "end_time":     "",
                                  "hostname":     "archcnstcm4381",
                                  "server_ip":    "10.10.11.17",
                                  "status":       "pause",
                                  "task_id":      "11277dd0-8c53-4fdd-8401-e9521e7dbae1",
                                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                                  "process":      0.000000
                          }, {
                                  "object_id":    "08d68a81-80e1-4827-a487-bd30ba801e20",
                                  "start_time":   "2022-Aug-30 19:28:28",
                                  "end_time":     "2022-Aug-30 19:29:55",
                                  "hostname":     "archcnstcm4381",
                                  "server_ip":    "10.10.11.17",
                                  "status":       "pause",
                                  "task_id":      "77b9b851-2459-4716-8b33-e9440a122e57",
                                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                                  "process":      0.000000
                          }]
          }
  }
  
  ~/zy # protector-client-py resume-task --task-id "f657004c-5d74-427a-9e2b-2ced15d26d61"
  {
          "status":       0,
          "data": {
                  "task_id":      "f657004c-5d74-427a-9e2b-2ced15d26d61",
                  "type": "create-nas-backup"
          }
  }
  
  ~/zy # protector-client-py show-task --task-id "f657004c-5d74-427a-9e2b-2ced15d26d61"
  {
          "status":       0,
          "data": {
                  "name": "1661858908",
                  "task_id":      "f657004c-5d74-427a-9e2b-2ced15d26d61",
                  "type": "create-nas-backup",
                  "backup_id":    "ff380431-b78c-4340-9c58-956bf4b9528a",
                  "object_id":    "97ce1470-b190-485a-8a3c-842d51756ed3",
                  "start_time":   "2022-Aug-30 19:28:28",
                  "end_time":     "2022-Aug-30 19:34:51",
                  "hostname":     "archcnstcm4381",
                  "server_ip":    "10.10.11.14",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "f657004c-5d74-427a-9e2b-2ced15d26d61",
                  "status":       "complete",
                  "process":      1.000000,
                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                  "error_code":   "",
                  "error_message":        "",
                  "sub_tasks":    [{
                                  "object_id":    "7fb00509-c74e-4872-8bc2-e3047fa9a230",
                                  "start_time":   "2022-Aug-30 19:28:28",
                                  "end_time":     "2022-Aug-30 19:34:51",
                                  "hostname":     "archcnstcm4381",
                                  "server_ip":    "10.10.11.14",
                                  "status":       "complete",
                                  "task_id":      "11277dd0-8c53-4fdd-8401-e9521e7dbae1",
                                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                                  "process":      1.000000
                          }, {
                                  "object_id":    "08d68a81-80e1-4827-a487-bd30ba801e20",
                                  "start_time":   "2022-Aug-30 19:28:28",
                                  "end_time":     "2022-Aug-30 19:34:51",
                                  "hostname":     "archcnstcm4381",
                                  "server_ip":    "10.10.11.14",
                                  "status":       "complete",
                                  "task_id":      "77b9b851-2459-4716-8b33-e9440a122e57",
                                  "description":  "create backup: 97ce1470-b190-485a-8a3c-842d51756ed3 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4381",
                                  "process":      1.000000
                          }]
          }
  }
  
  ```
-
-
- 12 kill transport
- ```
  [root@archcnstcm4380 ~]# docker restart xport1.4
  xport1.4
  
  ~/zy # protector-client-py show-task --task-id "baa2deda-a9d8-4573-bc05-c8b74881d097"
  {
          "status":       0,
          "data": {
                  "name": "1661859697",
                  "task_id":      "baa2deda-a9d8-4573-bc05-c8b74881d097",
                  "type": "create-nas-backup",
                  "backup_id":    "1beaf960-0261-47fc-8175-517672ccf360",
                  "object_id":    "ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e",
                  "start_time":   "2022-Aug-30 19:41:37",
                  "end_time":     "",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "baa2deda-a9d8-4573-bc05-c8b74881d097",
                  "status":       "pause",
                  "process":      0.170000,
                  "description":  "create backup: ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                  "error_code":   "ArP1083E_trans_task_id_not_exist",
                  "error_message":        "task id not found in transport record {\"taskId\":\"baa2deda-a9d8-4573-bc05-c8b74881d097\"}",
                  "sub_tasks":    [{
                                  "object_id":    "c776616a-f99b-4688-bf6d-f3d58ace782c",
                                  "start_time":   "2022-Aug-30 19:41:37",
                                  "end_time":     "",
                                  "hostname":     "archcnstcm4380",
                                  "server_ip":    "10.10.11.11",
                                  "status":       "pause",
                                  "task_id":      "ef968c98-a1d7-49fa-86f5-6588d398abe6",
                                  "description":  "create backup: ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                                  "process":      0.000000
                          }]
          }
  }
  
  ```
- 13 kill transport
- ```
  [root@archcnstcm4381 ~]# docker restart xport1.4
  xport1.4
  
  ~/zy # protector-client-py show-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "name": "1661858589",
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup",
                  "backup_id":    "2e36b55f-8204-470d-84d1-eb543458f891",
                  "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                  "start_time":   "2022-Aug-30 19:23:09",
                  "end_time":     "2022-Aug-30 19:29:55",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.17",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "706bf418-8c2b-473a-a234-46af893cd791",
                  "status":       "pause",
                  "process":      0.750000,
                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                  "error_code":   "ArP1083E_trans_task_id_not_exist",
                  "error_message":        "task id not found in transport record {\"taskId\":\"706bf418-8c2b-473a-a234-46af893cd791\"}",
                  "sub_tasks":    [{
                                  "object_id":    "8034d669-cb71-4f79-b453-9adbc308e7ff",
                                  "start_time":   "2022-Aug-30 19:23:09",
                                  "end_time":     "2022-Aug-30 19:29:55",
                                  "hostname":     "archcnstcm4380",
                                  "server_ip":    "10.10.11.17",
                                  "status":       "pause",
                                  "task_id":      "2f3e31d8-1bf5-40f3-ae80-b6fe5f4f1f10",
                                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                                  "process":      0.000000
                          }]
          }
  }
  
  ```
- 13 resume task
- ```
  ~/zy # protector-client-py resume-task --task-id "baa2deda-a9d8-4573-bc05-c8b74881d097"
  {
          "status":       0,
          "data": {
                  "task_id":      "baa2deda-a9d8-4573-bc05-c8b74881d097",
                  "type": "create-nas-backup"
          }
  }
  ~/zy #
  ~/zy #
  ~/zy # protector-client-py show-task --task-id "baa2deda-a9d8-4573-bc05-c8b74881d097"
  {
          "status":       0,
          "data": {
                  "name": "1661859697",
                  "task_id":      "baa2deda-a9d8-4573-bc05-c8b74881d097",
                  "type": "create-nas-backup",
                  "backup_id":    "1beaf960-0261-47fc-8175-517672ccf360",
                  "object_id":    "ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e",
                  "start_time":   "2022-Aug-30 19:41:37",
                  "end_time":     "2022-Aug-30 19:44:33",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.17",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "baa2deda-a9d8-4573-bc05-c8b74881d097",
                  "status":       "complete",
                  "process":      1.000000,
                  "description":  "create backup: ec5d6b7a-62b9-4d68-9dc6-61e2e52b834e to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                  "error_code":   "",
                  "error_message":        "",
                  "sub_tasks":    [{
                                  "object_id":    "c776616a-f99b-4688-bf6d-f3d58ace782c",
                                  "start_time":   "2022-Aug-30 19:41:37",
                                  "end_time":     "2022-Aug-30 19:44:32",
                                  "hostname":     "archcnstcm4380",
                                  "server_ip":    "10.10.11.17",
                                  "status":       "complete",
                                  "task_id":      "ef968c98-a1d7-49fa-86f5-6588d398abe6",
                                  "description":  "Backup size 0.",
                                  "process":      1.000000
                          }]
          }
  }
  
  ```
- 12 resume task
- ```
  ~/zy # protector-client-py resume-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup"
          }
  }
  
  
  ```
- 12 restart pserv
- ```
  [root@archcnstcm4380 ~]# docker restart pserv1.4
  ```
- 11 resume task
- ```
  ~/zy # protector-client-py show-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "name": "1661858589",
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup",
                  "backup_id":    "2e36b55f-8204-470d-84d1-eb543458f891",
                  "object_id":    "3d475dd0-6d60-4744-86d8-e78df2a8c789",
                  "start_time":   "2022-Aug-30 19:23:09",
                  "end_time":     "2022-Aug-30 19:46:57",
                  "hostname":     "archcnstcm4380",
                  "server_ip":    "10.10.11.11",
                  "source_type":  "ArcherOS_NAS",
                  "management_platform":  "CS",
                  "site_id":      "0b8db995-7a98-4493-a3ab-ac46a8ce746a",
                  "leader_task_id":       "706bf418-8c2b-473a-a234-46af893cd791",
                  "status":       "pause",
                  "process":      0.810000,
                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                  "error_code":   "ArP1114E_task_timeout_caused_by_node_down",
                  "error_message":        "ArP1114E task timeout as the node running the task was down.",
                  "sub_tasks":    [{
                                  "object_id":    "8034d669-cb71-4f79-b453-9adbc308e7ff",
                                  "start_time":   "2022-Aug-30 19:23:09",
                                  "end_time":     "2022-Aug-30 19:46:57",
                                  "hostname":     "archcnstcm4380",
                                  "server_ip":    "10.10.11.11",
                                  "status":       "pause",
                                  "task_id":      "2f3e31d8-1bf5-40f3-ae80-b6fe5f4f1f10",
                                  "description":  "create backup: 3d475dd0-6d60-4744-86d8-e78df2a8c789 to 0b8db995-7a98-4493-a3ab-ac46a8ce746a on host: archcnstcm4380",
                                  "process":      0.000000
                          }]
          }
  }
  
  ~/zy # protector-client-py resume-task --task-id "706bf418-8c2b-473a-a234-46af893cd791"
  {
          "status":       0,
          "data": {
                  "task_id":      "706bf418-8c2b-473a-a234-46af893cd791",
                  "type": "create-nas-backup"
          }
  }
  
  
  ```