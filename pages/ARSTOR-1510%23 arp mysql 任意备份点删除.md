title:: ARSTOR-1510# arp mysql 任意备份点删除

- ## 功能描述
- 因每次增量备份都会合到全量目录中，除了最后一次备份，其他只能删除对应快照，代表无法再恢复到此备份点。
- 删除对应快照点
- 如果是最后一个快照点，删除整个dataset
- 如果是最后一个备份点，rollback到前一次依赖的快照点
- ## 业务流程图
- ![delete backup.png](../assets/delete_backup_1659518157713_0.png)
- ## coding
	- DONE 参数校验
	  id:: 62ea3106-422b-40e0-aa99-77a85fdaaf9b
	  :LOGBOOK:
	  CLOCK: [2022-08-03 Wed 17:30:19]
	  CLOCK: [2022-08-03 Wed 17:30:25]--[2022-08-03 Wed 18:07:43] =>  00:37:18
	  :END:
	- DONE unit test
	  id:: 62ea30f3-5205-4f1a-b198-f6c35558fcbc
	  :LOGBOOK:
	  CLOCK: [2022-08-03 Wed 16:25:53]--[2022-08-03 Wed 18:07:56] =>  01:42:03
	  :END: