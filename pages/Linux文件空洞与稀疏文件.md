- [Linux文件空洞与稀疏文件-爱开源 (aikaiyuan.com)](https://www.aikaiyuan.com/8277.html)
- [(27条消息) KVM - qcow2 和 raw 格式对比_Suhw的博客-CSDN博客_qcow2 raw](https://blog.csdn.net/sssssuuuuu666/article/details/106999198)
- 文件空洞
	- 在UNIX文件操作中，文件位移量可以大于文件的当前长度。在这种情况下，对该文件的下一次写将延长该文件，并在文件中构成一个空洞。位于文件中没有被写过的字节都被设为0。
	- 如果offset比文件的当前长度更大，下一个写操作就会把文件撑大（extend），在文件里创造”空洞（hole）“。没有被实际写入文件的所有字节由重复的0表示。空洞是否占用硬盘空间由文件系统决定。
	  ![20140913144536_1](https://pic.aikaiyuan.com/wp-content/uploads/2014/09/20140913144536_1.jpg)
- 稀疏文件（Sparse File）
	- 稀疏文件与其他普通文件相同，区别在于文件中的部分数据全0，且这部分数据不占用磁盘空间。
	- 稀疏文件的创建与查看
	  ```
	  [root@localhost ~]# dd if=/dev/zero of=sparse-file bs=1 count=1 seek=1024k
	  [root@localhost ~]# ls -l sparse-file
	  -rw-r--r-- 1 root root 1048577 Oct 15 17:50 sparse-file
	  [root@localhost ~]# du -sh sparse-file
	  8.0K sparse-file
	  [root@localhost ~]# cat anaconda-ks.cfg >> sparse-file
	  [root@localhost ~]# du -sh sparse-file
	  12K sparse-file
	  [root@localhost ~]# du -sh anaconda-ks.cfg
	  12K anaconda-ks.cfg
	  [root@localhost ~]#
	  ```
-