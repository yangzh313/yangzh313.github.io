- ## 压缩测试
- ### dataset开启压缩
- #### 备份
	- rsync不压缩
	  ```
	  sent 2,652,461,286 bytes  received 6,112 bytes  43,129,551.19 bytes/sec
	  total size is 2,651,789,040  speedup is 1.00
	  
	  real    1m0.394s
	  user    0m14.379s
	  sys     0m11.729s
	  
	  ```
	- rsync压缩
	  ```
	  sent 1,104,410,304 bytes  received 6,112 bytes  4,535,591.03 bytes/sec
	  total size is 2,651,789,040  speedup is 2.40
	  
	  real    4m3.081s
	  user    3m44.103s
	  sys     0m12.046s
	  
	  ```
- #### 恢复
- ```
  [root@zymysql mnt]# mount -t nfs 10.142.192.69:/mnt/zpool/site-207157dc-38fd-4295-9fd7-108c17b98a0d/b775c1c0-86d3-4445-9053-2b4f618c4832/0717c3ba-4ce9-40ca-9667-2ad2e2ae06a3/3306/1660540817 test/
  [root@zymysql mnt]#
  [root@zymysql mnt]# cd test/
  [root@zymysql test]# ll
  total 12
  drwxr-x---. 6 root nfsnobody 16 Aug 15 13:21 full
  [root@zymysql test]#
  [root@zymysql test]# du -sh
  1.8G    .
  ```
- **rsync压缩**
- ```
  [root@zymysql test]# rsync -avzP full/* /tmp/test
  sent 1,104,293,733 bytes  received 5,752 bytes  5,399,997.48 bytes/sec
  total size is 2,551,047,576  speedup is 2.31
  ```
- **rsync不压缩**
- ```
  sent 2,551,694,117 bytes  received 5,784 bytes  108,582,974.51 bytes/sec
  total size is 2,551,047,576  speedup is 1.00
  ```
- ### dataset关闭压缩
- #### 备份
- rysnc不压缩
- ```
  [root@zymysql mydata]# time rsync -avP 3307/* /mnt/test/test
  sent 2,652,461,286 bytes  received 6,112 bytes  43,842,436.33 bytes/sec
  total size is 2,651,789,040  speedup is 1.00
  
  real    0m59.946s
  user    0m14.163s
  sys     0m10.429s
  
  ```
- rsync压缩
- ```
  [root@zymysql mydata]# time rsync -avzP 3307/* /mnt/test/test
  sent 1,104,410,304 bytes  received 6,112 bytes  4,630,676.80 bytes/sec
  total size is 2,651,789,040  speedup is 2.40
  
  real    3m58.035s
  user    3m43.424s
  sys     0m11.856s
  
  ```
- #### 恢复
- ```
  # rsync压缩
  sent 1,104,293,733 bytes  received 5,784 bytes  5,347,697.42 bytes/sec
  total size is 2,551,047,576  speedup is 2.31
  # rsync不压缩
  sent 2,551,694,117 bytes  received 5,784 bytes  104,151,016.37 bytes/sec
  total size is 2,551,047,576  speedup is 1.00
  
  ```
-
- ## [[ARSTOR-1517 限速测试]]
-