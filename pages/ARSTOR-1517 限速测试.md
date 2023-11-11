- [[Sep 2nd, 2022]]
- ## backup
- --throttle=1
- ```
  zymysql                    => 10.142.192.38              80.6Mb  82.8Mb  74.8Mb
                             <=                             539Kb   558Kb   461Kb
  ```
- --throttle=2
- ```
                  1.86Gb          3.73Gb          5.59Gb          7.45Gb    9.31Gb
  └───────────────┴───────────────┴───────────────┴───────────────┴───────────────
  zymysql                    => 10.142.192.38               191Mb   161Mb   160Mb
                             <=                             755Kb   661Kb   777Kb
  
  ```
- ## prepare
- > --use-memory=#
  This option affects how much memory is allocated for preparing a backup with xtrabackup --prepare, or
  analyzing statistics with xtrabackup --stats. Its purpose is similar to innodb_buffer_pool_size. It does
  not do the same thing as the similarly named option in Oracle’s InnoDB Hot Backup tool. The default value is
  100MB, and if you have enough available memory, 1GB to 2GB is a good recommended value. Multiples are
  supported providing the unit (e.g. 1MB, 1M, 1GB, 1G)
- 增量大小：`8.6G    /mnt/test/incr`
- ### --use-memory=100MB(default)
	- ```
	                  1.86Gb          3.73Gb          5.59Gb          7.45Gb    9.31Gb
	  └───────────────┴───────────────┴───────────────┴───────────────┴───────────────
	  zymysql                    => truenas                    58.5Mb  44.1Mb  44.1Mb
	                             <=                            1.20Mb  38.2Mb  39.4Mb
	  
	  ```
	- 总流量
	  ```
	                  1.86Gb          3.73Gb          5.59Gb          7.45Gb    9.31Gb
	  └───────────────┴───────────────┴───────────────┴───────────────┴───────────────
	  zymysql                    => 10.25.2.24                  352b    429b    453b
	                             <=                             184b    184b    202b
	  zymysql                    => truenas                       0b      0b     10b
	                             <=                               0b      0b     10b
	  
	  ────────────────────────────────────────────────────────────────────────────────
	  TX:             cum:   11.5GB   peak:    688b   rates:    352b    429b    463b
	  RX:                    11.3GB            536b             184b    184b    212b
	  TOTAL:                 22.8GB           1.15Kb            536b    613b    675b
	  ```
	  ```
	  220902 17:49:38 [01] Copying /mnt/test/incr/mysql/columns_priv.MYI to ./mysql/co       lumns_priv.MYI
	  220902 17:49:42 completed OK!
	  
	  real    31m26.535s
	  user    0m2.432s
	  sys     0m21.171s
	  ```
- ### --use-memory=1G
	- ```
	  [root@zymysql ~]# xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --target-dir=/mnt/test/full --apply-log-only
	  
	  [root@zymysql ~]# xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --incremental-dir=/mnt/test/incr --target-dir=/mnt/test/full --use-memory=1G
	  
	  zymysql                    => truenas                    57.8Mb  54.5Mb  52.5Mb
	                             <=                            1.20Mb  52.5Mb  52.5Mb
	                             
	      ────────────────────────────────────────────────────────────────────────────────
	  TX:             cum:   11.5GB   peak:    640b   rates:      0b    333b    389b
	  RX:                    11.4GB            536b               0b    147b    183b
	  TOTAL:                 22.9GB           1.15Kb              0b    480b    572b
	  
	  220902 18:27:29 [01] Copying /mnt/test/incr/mysql/columns_priv.MYI to ./mysql/co       lumns_priv.MYI
	  220902 18:27:34 completed OK!
	  ```
- ### prepare到本地, 100M
	- ```
	  [root@zymysql ~]# xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --target-dir=/home/mysql-test/full --apply-log-only
	  
	  [root@zymysql ~]# time xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --incremental-dir=/home/mysql-test/incr --target-dir=/home/mysql-test/full
	  
	  real    15m41.684s
	  user    0m2.569s
	  sys     1m8.564s
	  
	  10MB/s左右
	  
	  [root@zymysql ~]# time cp -r /home/mysql-test /mnt/test/
	  
	  real    4m8.254s
	  user    0m0.043s
	  sys     0m29.316s
	  
	  ```
- ### prepare到本地, 1G
	- ```
	  [root@zymysql ~]# xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --target-dir=/home/mysql-test/full --apply-log-only
	  
	  [root@zymysql ~]# time xtrabackup --defaults-file=/etc/my.cnf --prepare --user=root --password= --incremental-dir=/home/mysql-test/incr --target-dir=/home/mysql-test/full
	  
	  real    15m35.166s
	  user    0m2.761s
	  sys     1m9.700s
	  
	  ```
- ## restore
- ### --bwlimit
- rsync -avrP --bwlimit 10240 /mnt/truenas_share/dd1 /data/nas/
- ```
                  19.1Mb          38.1Mb          57.2Mb          76.3Mb    95.4Mb
  └───────────────┴───────────────┴───────────────┴───────────────┴───────────────
  zymysql                    => 10.142.192.38               181Kb   180Kb   174Kb
                             <=                            75.2Mb  78.2Mb  78.5Mb
  
  ```