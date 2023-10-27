- [On Redhat, for NFS over TCP, the default timeo=600, retrans=2, so the major timeout value = timeo + timeo * retrans=600+600*2=1800=180s.](https://www.howtouselinux.com/post/understanding-timeo-in-nfs)
- test xtrabackup
	- timeo 600,retrans 3-8mins
	  ```
	  220831 15:49:35 >> log scanned up to (2843182890)
	  4
	  Aug 31 15:53:34 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  1
	  Aug 31 15:54:34 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  3
	  Aug 31 15:57:34 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  exit
	  ```
	- timeo 100,retrans 2-1mins
		- ```
		  220831 16:47:39 >> log scanned up to (2843182890)
		  
		  220831 16:48:08 >> log scanned up to (2843182890)
		  xtrabackup: Can't create directory '/mnt/test/full/mysql/' (Errcode: 5 - Input/output error)
		  [01] error: cannot open the destination stream for mysql/help_keyword.frm
		  [01] Error: copy_file() failed.
		  Failed to copy file ./mysql/help_keyword.frm
		  0.5
		  Aug 31 16:48:08 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 16:48:38 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  exit
		  ```
	- timeo 200,retrans 2-2mins
		- ```
		  220831 16:43:26 >> log scanned up to (2843182890)
		  
		  220831 16:44:26 >> log scanned up to (2843182890)
		  xtrabackup: Can't create directory '/mnt/test/full/mysql/' (Errcode: 5 - Input/output error)
		  [01] error: cannot open the destination stream for mysql/time_zone_leap_second.frm
		  [01] Error: copy_file() failed.
		  Failed to copy file ./mysql/time_zone_leap_second.frm
		  1
		  Aug 31 16:44:26 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 16:45:26 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  exit
		  ```
	- timeo 300,retrans 2-4.5mins
		- ```
		  220831 16:33:23 >> log scanned up to (2843182890)
		  1.5
		  Aug 31 16:34:53 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  1
		  Aug 31 16:35:46 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  220831 16:36:22 >> log scanned up to (2843182890)
		  xtrabackup: Can't create directory '/mnt/test/full/mysql/' (Errcode: 5 - Input/output error)
		  [01] error: cannot open the destination stream for mysql/servers.frm
		  [01] Error: copy_file() failed.
		  Failed to copy file ./mysql/servers.frm
		  0.5
		  Aug 31 16:36:23 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 16:37:17 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 16:37:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  1.5
		  Aug 31 16:37:53 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  
		  ```
	- timeo 600,retrans 2-7mins
		- ```
		  - 220831 14:37:22 >> log scanned up to (2843182890)
		  3
		  - Aug 31 14:40:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		  - Aug 31 14:41:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		  - Aug 31 14:43:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		    Aug 31 14:44:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		    Aug 31 14:44:27 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		  - 220831 14:43:22 >> log scanned up to (2843182890)  
		    xtrabackup: Can't create directory '/mnt/test/full/sys/' (Errcode: 5 - Input/output error)  
		    [01] error: cannot open the destination stream for sys/statements_with_errors_or_warnings.frm  
		    [01] Error: copy_file() failed.  
		    Failed to copy file ./sys/statements_with_errors_or_warnings.frm  
		  - Aug 31 14:44:27 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out  
		    Aug 31 14:46:22 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  ```
	- timeo 1000,retrans 2-15mins
		- ```
		  220831 16:54:06 >> log scanned up to (2843182890)
		  5
		  Aug 31 16:59:06 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  1
		  Aug 31 17:00:26 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  4
		  Aug 31 17:04:07 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  xtrabackup: Can't create directory '/mnt/test/full/mysql/' (Errcode: 5 - Input/output error)
		  [01] error: cannot open the destination stream for mysql/db.frm
		  [01] Error: copy_file() failed.
		  Failed to copy file ./mysql/db.frm
		  1.5
		  Aug 31 17:05:26 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:05:31 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  
		  Aug 31 17:09:07 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  exit
		  
		  test 2
		  2022-09-06 20:18:50     INFO    common/utils.go:178     start exec rsync -avrP --bwlimit=1024 /opt/mysqlmount/mount/3307/1660714729713-d04ccd25-5cb8-4e54-ad0f-9d1d89e68e3e/full/* /mysql/mydata/3307
		  
		  ```
	- timeo 3000,retrans 2-mins
		- ```
		  ```
	- timeo 6000,retrans 2-31mins
		- ```
		  220831 17:24:44 >> log scanned up to (2843182890)
		  10
		  Aug 31 17:34:44 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  1
		  Aug 31 17:35:44 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:44:45 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:45:46 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:46:16 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:54:53 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:55:53 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  Aug 31 17:55:53 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
		  
		  ```
	- timeo 12000,retrans 2- >1h
		- ```
		  220901 14:27:44 >> log scanned up to (2843182890)
		  10
		  Sep  1 14:37:46 zymysql kernel: nfs: server 10.142.192.69 not responding, timed out
		  Sep  1 14:37:46 zymysql kernel: nfs: server 10.142.192.69 not responding, timed out
		  Sep  1 14:37:46 zymysql kernel: nfs: server 10.142.192.69 not responding, timed out
		  
		  220901 15:30:16 >> log scanned up to (2843182890)
		  xtrabackup: Can't create directory '/mnt/test/full/test/' (Errcode: 5 - Input/output error)
		  [01] xtrabackup: error: cannot open the destination stream for test/sbtest9.ibd
		  [01] xtrabackup: Error: xtrabackup_copy_datafile() failed.
		  [01] xtrabackup: Error: failed to copy datafile.
		  Thu Sep  1 15:30:16 CST 2022
		  
		  test again:
		  
		  
		  ```
	- timeo 600, retrans 1-6mins
	  ```
	  220831 15:17:44 >> log scanned up to (2843182890)
	  2
	  Aug 31 15:19:43 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  Aug 31 15:20:40 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  220831 15:21:43 >> log scanned up to (2843182890)
	  xtrabackup: Can't create directory '/mnt/test/full/sys/' (Errcode: 5 - Input/output error)
	  [01] error: cannot open the destination stream for sys/metrics.frm
	  [01] Error: copy_file() failed.
	  Failed to copy file ./sys/metrics.frm
	  
	  Aug 31 15:21:43 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:22:40 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:22:45 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  Aug 31 15:23:43 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  exit
	  ```
	- timeo 600, retrans 0-4mins
	  ```
	  220831 15:02:51 >> log scanned up to (2843182890)
	  1
	  220831 15:03:51 >> log scanned up to (2843182890)
	  xtrabackup: Can't sync file '/mnt/test/full/mysql/tables_priv.MYI' to disk (Errcode: 5 - Input/output error)
	  Aug 31 15:03:51 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  Aug 31 15:04:35 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:04:51 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:05:35 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:05:40 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:05:51 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  xtrabackup: Can't create directory '/mnt/test/full/mysql/' (Errcode: 5 - Input/output error)
	  [01] error: cannot open the destination stream for mysql/tables_priv.frm
	  [01] Error: copy_file() failed.
	  Failed to copy file ./mysql/tables_priv.frm
	  
	  Aug 31 15:06:35 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:06:40 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:06:45 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  Aug 31 15:06:51 zymysql kernel: nfs: server 10.142.192.38 not responding, timed out
	  
	  exit
	  ```
	-
-