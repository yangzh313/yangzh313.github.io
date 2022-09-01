title:: [ERROR] InnoDB: Unable to lock ./ibdata1 error: 37

- ## Problem
	- ```
	  2022-08-09T05:53:48.392303Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
	  2022-08-09T05:53:48.397348Z 0 [Note] mysqld (mysqld 5.7.37-log) starting as process 41679 ...
	  2022-08-09T05:53:48.410089Z 0 [Note] InnoDB: PUNCH HOLE support available
	  2022-08-09T05:53:48.410159Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
	  2022-08-09T05:53:48.410170Z 0 [Note] InnoDB: Uses event mutexes
	  2022-08-09T05:53:48.410175Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
	  2022-08-09T05:53:48.410196Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
	  2022-08-09T05:53:48.410202Z 0 [Note] InnoDB: Using Linux native AIO
	  2022-08-09T05:53:48.411497Z 0 [Note] InnoDB: Number of pools: 1
	  2022-08-09T05:53:48.411768Z 0 [Note] InnoDB: Using CPU crc32 instructions
	  2022-08-09T05:53:48.415384Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
	  2022-08-09T05:53:48.429246Z 0 [Note] InnoDB: Completed initialization of buffer pool
	  2022-08-09T05:53:48.433019Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
	  ```
- ## Try
	- 复制到本地运行
		- [root@zymysql ar-dbagent]# cp -a /mnt/mount/3307/1660032414-f34f1c12-f21f-405a-a0f7-f0ff3416d806/full /mysql/mydata/
		  
		  bash -c "/usr/sbin/mysqld --datadir=/mysql/mydata/3307 --user=root  --socket=/mysql/mydata/3307/mysql.sock  --log-error=/mysql/mydata/3307/mysqld-3307.log --pid-file=/mysql/mydata/3307/mysql.pid --port=3307"
		  
		  [root@zymysql mydata]# netstat -ntlp|grep 3307
		  tcp6       0      0 :::3307                 :::*                    LISTEN      115836/mysqld
- bash -c "/usr/sbin/mysqld --datadir=/mysql/mydata/3307 --port=3307"
- ## Reason
	- When InnoDB starts, it opens the system tablespace file(s) (the ibdataX files) in read/write mode and requests an exclusive filesystem lock. This prevents multiple instances of MySQL/InnoDB from starting and using the same system tablespace files, which would corrupt the data. Sometimes the file may be ib_logfile instead of ibdata1, or one of the other key InnoDB files.
	  NFS (prior to version 4) uses lazy locking, so when MySQL/InnoDB is not properly shutdown on the previous node/instance the previous lock is still around on the NFS server. You have to actually tell NFS that you are releasing a lock (managed by lockd/statd and sm-notify on the NFS host/server and the peer/client). This is one of the things that has been greatly improved in NFSv4 (with the addition/introduction of advisory and lease based locking).
	-
- ## Workaround
	- `systemctl restart nfslock`
	- or enable nfs-v4
	- Add nolock option into nfs mount entry, `mount -o nfslock`
	  id:: 62f9ac4b-50bd-4c3c-adf9-c2b576b3e623
- ## Reference
	- [[MySQL:[ERROR] InnoDB: Unable to lock ./ibdata1 error: 11 特殊错误一列 - 墨天轮]]
	- [My life with BigData: Error 37 and 11 on a MYSQL (CentOS) after we moved data to nfs. (bigdatadk.blogspot.com)](http://bigdatadk.blogspot.com/2016/06/error-37-and-11-on-mysql-after-we-moved.html)
	- [**How to clear NFS locks during network crash or outage for Oracle datafiles.**](http://harishdixit.blogspot.com/2010/11/how-to-clear-nfs-locks-during-network.html)
	-
	-