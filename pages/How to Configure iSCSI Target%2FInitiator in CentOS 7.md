- 参考
	- [How to Configure iSCSI Target/Initiator in CentOS 7 | CentLinux](https://www.centlinux.com/2018/08/configure-iscsi-target-initiator-centos-7.html)
	  [Configure iSCSI Target & Initiator on CentOS 7 / RHEL7 (itzgeek.com)](https://www.itzgeek.com/how-tos/linux/centos-how-tos/configure-iscsi-target-initiator-on-centos-7-rhel7.html)
	  [iSCSI的基本架构及操作简介 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/60986068) #iSCSI
	- [Configure iSCSI Target & Initiator on CentOS 7 / RHEL7 (itzgeek.com)](https://www.itzgeek.com/how-tos/linux/centos-how-tos/configure-iscsi-target-initiator-on-centos-7-rhel7.html)
	- [How to Configure iSCSI Target on Red Hat Enterprise Linux 7 (atodorov.org)](http://atodorov.org/blog/2015/04/07/how-to-configure-iscsi-target-on-red-hat-enterprise-linux-7/)
- 环境
	- server：10.142.144.155
	- client：10.142.144.34
- 配置iSCSI Target
	- 创建分区
		- ```
		  [root@node2 ~]# fdisk /dev/vdb
		  Welcome to fdisk (util-linux 2.23.2).
		  
		  Changes will remain in memory only, until you decide to write them.
		  Be careful before using the write command.
		  
		  Device does not contain a recognized partition table
		  Building a new DOS disklabel with disk identifier 0xbbe388b5.
		  
		  Command (m for help): n
		  Partition type:
		     p   primary (0 primary, 0 extended, 4 free)
		     e   extended
		  Select (default p):
		  Using default response p
		  Partition number (1-4, default 1):
		  First sector (2048-209715199, default 2048):
		  Using default value 2048
		  Last sector, +sectors or +size{K,M,G} (2048-209715199, default 209715199): +10G
		  Partition 1 of type Linux and of size 10 GiB is set
		  
		  Command (m for help): w
		  The partition table has been altered!
		  
		  Calling ioctl() to re-read partition table.
		  Syncing disks.
		  [root@node2 ~]#
		  [root@node2 ~]#
		  [root@node2 ~]# lsblk
		  NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
		  sr0              11:0    1 1024M  0 rom
		  vda             252:0    0  100G  0 disk
		  ├─vda1          252:1    0    1G  0 part /boot
		  └─vda2          252:2    0   99G  0 part
		    ├─centos-root 253:0    0   50G  0 lvm  /
		    ├─centos-swap 253:1    0  7.9G  0 lvm
		    └─centos-home 253:2    0 41.1G  0 lvm  /home
		  vdb             252:16   0  100G  0 disk
		  └─vdb1          252:17   0   10G  0 part
		  
		  ```
	- 安装**targetcli**包
		- ```
		  yum install -y targetcli
		  ```
	- 创建**Storage Block**
		- [root@node2 ~]# targetcli
		  Warning: Could not load preferences file /root/.targetcli/prefs.bin.
		  targetcli shell version 2.1.53
		  Copyright 2011-2013 by Datera, Inc and others.
		  For help on commands, type 'help'.
		  
		  /> ls
		  o- / ............................................................................. [...]
		    o- backstores .................................................................. [...]
		    | o- block ...................................................... [Storage Objects: 0]
		    | o- fileio ..................................................... [Storage Objects: 0]
		    | o- pscsi ...................................................... [Storage Objects: 0]
		    | o- ramdisk .................................................... [Storage Objects: 0]
		    o- iscsi ................................................................ [Targets: 0]
		    o- loopback ............................................................. [Targets: 0]
		  /> cd backstores/block
		  /backstores/block> create block1 /dev/vdb1
		  Created block storage object block1 using /dev/vdb1.
		  /backstores/block> ls
		  o- block .......................................................... [Storage Objects: 1]
		    o- block1 ............................... [/dev/vdb1 (10.0GiB) write-thru deactivated]
		      o- alua ........................................................... [ALUA Groups: 1]
		        o- default_tg_pt_gp ............................... [ALUA state: Active/optimized]
	- Create a **TPG** (Target Portal Group).
		- ```
		  /backstores/block> cd /iscsi
		  /iscsi> create iqn.2018-07.com.example.server1:remotedisk1
		  Created target iqn.2018-07.com.example.server1:remotedisk1.
		  Created TPG 1.
		  Global pref auto_add_default_portal=true
		  Created default portal listening on all IPs (0.0.0.0), port 3260.
		  ```
	- 获取客户端的iqn，创建ACL限制对target的访问
		- ```
		  [root@node1 ~]# cat /etc/iscsi/initiatorname.iscsi
		  InitiatorName=iqn.1994-05.com.redhat:25fbfb1a9559
		  /iscsi> cd /iscsi/iqn.2018-07.com.example.server1:remotedisk1/tpg1/acls
		  /iscsi/iqn.20...sk1/tpg1/acls>
		  /iscsi/iqn.20...sk1/tpg1/acls> create iqn.1994-05.com.redhat:25fbfb1a9559
		  Created Node ACL for iqn.1994-05.com.redhat:25fbfb1a9559
		  /iscsi/iqn.20...sk1/tpg1/acls> ls
		  o- acls ...................................................................... [ACLs: 1]
		    o- iqn.1994-05.com.redhat:25fbfb1a9559 .............................. [Mapped LUNs: 0]
		  ```
	- 创建**LUN** (Logical Unit Number).
		- ```
		  /iscsi/iqn.20...sk1/tpg1/acls> cd /iscsi/iqn.2018-07.com.example.server1:remotedisk1/tpg1/luns
		  /iscsi/iqn.20...sk1/tpg1/luns> create /backstores/block/block1
		  Created LUN 0.
		  Created LUN 0->0 mapping in node ACL iqn.1994-05.com.redhat:25fbfb1a9559
		  /iscsi/iqn.20...sk1/tpg1/luns> ls
		  o- luns ...................................................................... [LUNs: 1]
		    o- lun0 ................................ [block/block1 (/dev/vdb1) (default_tg_pt_gp)]
		  ```
	- 创建**Portal**
		- ```
		  /iscsi/iqn.20...sk1/tpg1/luns> cd /iscsi/iqn.2018-07.com.example.server1:remotedisk1/tpg1/portals/
		  /iscsi/iqn.20.../tpg1/portals> create 10.142.144.155
		  Using default IP port 3260
		  Could not create NetworkPortal in configFS
		  /iscsi/iqn.20.../tpg1/portals> delete 0.0.0.0 3260
		  Deleted network portal 0.0.0.0:3260
		  /iscsi/iqn.20.../tpg1/portals> create 10.142.144.155
		  Using default IP port 3260
		  Created network portal 10.142.144.155:3260.
		  /iscsi/iqn.20.../tpg1/portals> exit
		  Global pref auto_save_on_exit=true
		  Configuration saved to /etc/target/saveconfig.json
		  ```
		- [iscsi报错：Could not create NetworkPortal in configFS - 代码先锋网 (codeleading.com)](https://www.codeleading.com/article/96702405107/#:~:text=Could%20not%20create%20NetworkPortal%20in%20configFS%20%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%EF%BC%9A%20%E5%88%A0%E9%99%A4%E8%BF%99%E4%B8%AA%E8%87%AA%E5%8A%A8%E9%BB%98%E8%AE%A4%E7%94%9F%E6%88%90%E7%9A%840.0.0.%E7%AB%AF%E5%8F%A3,%2Fiscsi%2Fiqn.19...%2Ftpg1%2Fportals%3E%20delete%200.0.0.0%203260%20Deleted%20network%20portal%200.0.0.0%3A3260)
	- stop firewalld
	- Start and enable the **target** service.
		- ```
		  [root@node2 ~]# systemctl enable target ; systemctl start target
		  Created symlink from /etc/systemd/system/multi-user.target.wants/target.service to /usr/lib/systemd/system/target.service.
		  ```
- 配置iSCSI Initiator
	- 安装和发现target
		- [root@node1 ~]# yum install -y iscsi-initiator-utils
		  Package iscsi-initiator-utils-6.2.0.874-22.el7_9.x86_64 already installed and latest version
		  Nothing to do
		  [root@node1 ~]# systemctl enable iscsi ; systemctl start iscsi
		  [root@node1 ~]# iscsiadm -m discovery -t sendtargets -p 10.142.144.155
		  10.142.144.155:3260,1 iqn.2018-07.com.example.server1:remotedisk1
		  [root@node1 ~]# systemctl restart iscsi
		  
		  [root@node1 ~]#
		  [root@node1 ~]# lsblk
		  NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
		  sda               8:0    0   10G  0 disk
		  sr0              11:0    1 1024M  0 rom
		  vda             252:0    0  100G  0 disk
		  ├─vda1          252:1    0    1G  0 part /boot
		  └─vda2          252:2    0   99G  0 part
		    ├─centos-root 253:0    0   50G  0 lvm  /
		    ├─centos-swap 253:1    0  7.9G  0 lvm
		    └─centos-home 253:2    0 41.1G  0 lvm  /home
		  vdb             252:16   0  100G  0 disk
		  └─vdb1          252:17   0  100G  0 part
		  vdc             252:32   0  100G  0 disk
		  └─vdc1          252:33   0  100G  0 part
		  loop0             7:0    0  114M  1 loop /var/lib/snapd/snap/core/13425
		  loop1             7:1    0  9.8M  1 loop /var/lib/snapd/snap/throttle/1
	- 对/dev/sda创建分区和文件系统
		- [root@node1 ~]# fdisk /dev/sda
		  Welcome to fdisk (util-linux 2.23.2).
		  
		  Changes will remain in memory only, until you decide to write them.
		  Be careful before using the write command.
		  
		  Device does not contain a recognized partition table
		  Building a new DOS disklabel with disk identifier 0xd62622dc.
		  
		  Command (m for help): n
		  Partition type:
		     p   primary (0 primary, 0 extended, 4 free)
		     e   extended
		  Select (default p):
		  Using default response p
		  Partition number (1-4, default 1):
		  First sector (2048-20971519, default 2048):
		  Using default value 2048
		  Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519):
		  Using default value 20971519
		  Partition 1 of type Linux and of size 10 GiB is set
		  
		  Command (m for help): w
		  The partition table has been altered!
		  
		  Calling ioctl() to re-read partition table.
		  Syncing disks.
		  [root@node1 ~]# fdisk -l /dev/sda
		  
		  Disk /dev/sda: 10.7 GB, 10737418240 bytes, 20971520 sectors
		  Units = sectors of 1 * 512 = 512 bytes
		  Sector size (logical/physical): 512 bytes / 512 bytes
		  I/O size (minimum/optimal): 512 bytes / 512 bytes
		  Disk label type: dos
		  Disk identifier: 0xd62622dc
		  
		     Device Boot      Start         End      Blocks   Id  System
		  /dev/sda1            2048    20971519    10484736   83  Linux
		  [root@node1 ~]#
		  [root@node1 ~]# mkfs.xfs /dev/sda1
		  meta-data=/dev/sda1              isize=512    agcount=4, agsize=655296 blks
		           =                       sectsz=512   attr=2, projid32bit=1
		           =                       crc=1        finobt=0, sparse=0
		  data     =                       bsize=4096   blocks=2621184, imaxpct=25
		           =                       sunit=0      swidth=0 blks
		  naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
		  log      =internal log           bsize=4096   blocks=2560, version=2
		           =                       sectsz=512   sunit=0 blks, lazy-count=1
		  realtime =none                   extsz=4096   blocks=0, rtextents=0
	- 挂载分区到本地
		- ```
		  [root@node1 ~]# blkid /dev/sda1
		  /dev/sda1: UUID="f0d44e70-9a79-4f51-99db-dc8dc6b31836" TYPE="xfs"
		  [root@node1 ~]#
		  [root@node1 ~]# echo "UUID=f0d44e70-9a79-4f51-99db-dc8dc6b31836 /mnt/remotedisk1 xfs _netdev 0 0" >> /etc/fstab
		  [root@node1 ~]# mount -a
		  [root@node1 ~]# mount |grep /mnt/remotedisk1
		  /dev/sda1 on /mnt/remotedisk1 type xfs (rw,relatime,attr2,inode64,noquota,_netdev)
		  ```
	- 移除iSCSI存储
		- ```
		  [root@node1 ~]# umount /mnt/remotedisk1
		  [root@node1 ~]# iscsiadm -m node -T iqn.2018-07.com.example.server1:remotedisk1 -p 10.142.144.155 -u
		  Logging out of session [sid: 1, target: iqn.2018-07.com.example.server1:remotedisk1, portal: 10.142.144.155,3260]
		  Logout of [sid: 1, target: iqn.2018-07.com.example.server1:remotedisk1, portal: 10.142.144.155,3260] successful.
		  ```