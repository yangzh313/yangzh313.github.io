- 参考：
	- qcow2
		- [Qcow2原理与基于Qcow2的快照 - 蓝色魔兽 - 博客园 (cnblogs.com)](https://www.cnblogs.com/kvm-qemu/articles/14017061.html)
		- [Read QCow2 file using C (data recovery part 3) | Raddinox](https://raddinox.com/how-to-read-qcow2-image-file-using-c)
		- [(29条消息) Qemu虚拟机QCOW2格式镜像文件的组成部分及关键算法分析_LastRitter的博客-CSDN博客](https://blog.csdn.net/superyongzhe/article/details/126439060)
	- golang
		- [(29条消息) Golang 读写二进制文件方法（二）_go 读取二进制文件_路多辛的博客-CSDN博客](https://blog.csdn.net/luduoyuan/article/details/128810116)
- 基本概念
	- 扇区：sector，磁盘中读取写入的基本单位，一个扇区是512字节。
	- 簇：cluster，由相邻的多个扇区组成。文件在磁盘中存储是以簇（块）为单位，比如文件系统把4k字节作为一个簇。簇是不连续的，内核为了能索引到这些簇使用了inode。
	- inode：存放在磁盘上，内核一般会对inode进行缓存。
- 实现
	- [[qcow2真实地址转换为虚拟地址]]
	-