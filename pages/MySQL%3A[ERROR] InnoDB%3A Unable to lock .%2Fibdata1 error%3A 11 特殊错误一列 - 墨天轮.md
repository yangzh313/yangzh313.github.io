title:: MySQL:[ERROR] InnoDB: Unable to lock ./ibdata1 error: 11 特殊错误一列 - 墨天轮

- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.modb.pro](https://www.modb.pro/db/85218)
  
  > 一、问题一般来讲启动多个 mysqld 作用同一个数据文件会报这个错。但是这里不是。第一遇到这个问题，用户用的 n
  
  [![](https://oss-emcsprod-public.oss-cn-beijing.aliyuncs.com/image/indexlogo.png)](https://www.modb.pro/) / MySQL:[ERROR] InnoDB: Unable to lock ./ibdata1 error: 11 特殊错误一列
- ### 一、问题
  
  一般来讲启动多个 mysqld 作用同一个数据文件会报这个错。但是这里不是。第一遇到这个问题，用户用的 nas 存储数据文件，记录一下。mysql 重启后一直启动不了，即便重启 OS 也不不行。一直报错如下：
  
  ```
  2021-03-25T11:11:02.069545-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:03.070008-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:04.077948-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:05.079244-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:06.080286-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:07.081550-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:08.087447-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:09.089804-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:10.091393-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:11.093382-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:12.095409-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:13.096458-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:14.099403-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:15.101033-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  2021-03-25T11:11:16.103315-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
  
  
  ```
  
  MOS 有相关文章，说明了这个问题，并且给出了原因和解决办法如下：
  
  
  *When InnoDB starts, it opens the system tablespace file(s) (the ibdataX files) in read/write mode and requests an exclusive filesystem lock. This prevents multiple instances of MySQL/InnoDB from starting and using the same system tablespace files, which would corrupt the data. Sometimes the file may be ib_logfile instead of ibdata1, or one of the other key InnoDB files.
  
  NFS (prior to version 4) uses lazy locking, so when MySQL/InnoDB is not properly shutdown on the previous node/instance the previous lock is still around on the NFS server. You have to actually tell NFS that you are releasing a lock (managed by lockd/statd and sm-notify on the NFS host/server and the peer/client). This is one of the things that has been greatly improved in NFSv4 (with the addition/introduction of advisory and lease based locking).
  
  Statd and its helper sm-notify is the deamon and tool combination that typically handles NFS client/peer host reboots, so that it can handle the resulting stale locks and release the locks previously held by that peer/client, but it doesn't handle the situation for individual processes (mysqld in this case). You can read more about each on your system using:
  - man statd
  - man sm-notify
  There is a lot of documentation floating around out there on the general topic, for example:
  
  *   [File locking](http://docstore.mik.ua/orelly/networking_2ndEd/nfs/ch07_05.htm)
  *   [Troubleshooting the network lock manager](http://www.ibm.com/support/knowledgecenter/ssw_aix_72/com.ibm.aix.networkcomm/nfs_netlocktrblsh.htm)
  *   [](http://www.ibm.com/support/knowledgecenter/ssw_aix_72/com.ibm.aix.networkcomm/nfs_netlocktrblsh.htm)[How to clear NFS locks during network crash or outage for Oracle datafiles](http://harishdixit.blogspot.com/2010/11/how-to-clear-nfs-locks-during-network.html)
  
  We essentially need to do one of the following when using an NFS share (typically on a NAS) as the basis for some kind of HA or failover setup:
  
  1. Clear out all locks on the NFS server as if it was rebooted
  2. Restart statd on the previously active/primary cluster node so that the NFS server releases all locks held by that server
  3. Have statd on the newly active/primary cluster node send a notification to statd on the NFS server to release all locks held by the previously active/primary MySQL host.
  
  Some other possible workarounds:
  
  1. Restart lockd and statd on the NFS host
  2. Use sm_mon -l (if you'
  
  re using a netapp device)
  3. Restart statd or networking on the previously active host/primary host
  
  This error can also occur when only using a single machine with the datadir on an NFS share, particularly if the instance of mysqld was killed abruptly such as with "kill -9".
  
  If for some reason the locks can't be cleared, another workaround is to copy the file to another location and update my.cnf to point to the new location. Sometimes copying the file and then copying it back is sufficient.(注意这段)
  
  The underlying issues here are vastly improved in NFSv4 with the addition of advisory and lease based locking, so if you plan on using NFS with MySQL over the long term, you should look into moving your NFS setup to V4（注意这段）*
  
  最后拷贝一份完整的目录解决了，由公司同事孙旭综解决.
### 二、测试

下列代码上一个 read file lock，抄的网上的改了一下. 然后启动 mysqld

```
#include<unistd.h>
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<errno.h>

int main()
{
int fd,res;
struct flock region;

fd=open("/opt/mysql/mysql3306/data/ibdata1",O_RDONLY);

printf("test %ld\n",fd);

if(fd==-1)
  {
    printf("Open error.\n");
    exit(1);
  }

region.l_type=F_RDLCK;
region.l_whence=SEEK_SET;
region.l_start=0;
region.l_len=0;

if((res=fcntl(fd,F_SETLK,®ion))==-1)
  {
    printf("Set RDLOCK lock error.");
    printf("Linux errno is:%d\n", errno);
    exit(1);
  }
while(1)
{}

close(fd);
}

```

如果 mysqld 启动的情况下，这段代码会报错

```
Set RDLOCK lock error.Linux errno is:11


```

如果先上 RDLOCK 然后启动 mysqld 则报错：

```
2021-03-25T11:09:53.937801-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
2021-03-25T11:09:54.938667-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
2021-03-25T11:09:55.938983-05:00 0 [ERROR] InnoDB: Unable to lock ./ibdata1 error: 11


```

文件锁报错为 资源不可达
### 三、关于 lsof 中的 fd

这个字段实际上包含了文件锁如下：

```
a.out     17828         root    3rR     REG                8,5 524288000     754291 /opt/mysql/mysql3306/data/ibdata1
mysqld    17605         mysql   11uW     REG                8,5 524288000     754291 /opt/mysql/mysql3306/data/ibdata1

```

其中描述如下：

```
     FD         is the File Descriptor number of the file or:

                     cwd  current working directory;
                     Lnn  library references (AIX);
                     err  FD information error (see NAME column);
                     jld  jail directory (FreeBSD);
                     ltx  shared library text (code and data);
                     Mxx  hex memory-mapped type number xx.
                     m86  DOS Merge mapped file;
                     mem  memory-mapped file;
                     mmap memory-mapped device;
                     pd   parent directory;
                     rtd  root directory;
                     tr   kernel trace file (OpenBSD);
                     txt  program text (code and data);
                     v86  VP/ix mapped file;

                FD is followed by one of these characters, describing the mode under which the file is open:

                     r for read access;
                     w for write access;
                     u for read and write access;
                     space if mode unknown and no lock
                          character follows;
                     `-' if mode unknown and lock
                          character follows.

                The mode character is followed by one of these lock characters, describing the type of lock applied to the file:

                     N for a Solaris NFS lock of unknown type;
                     r for read lock on part of the file;
                     R for a read lock on the entire file;
                     w for a write lock on part of the file;
                     W for a write lock on the entire file;
                     u for a read and write lock of any length;
                     U for a lock of unknown type;
                     x for an SCO OpenServer Xenix lock on part      of the file;
                     X for an SCO OpenServer Xenix lock on the      entire file;
                     space if there is no lock.

                See the LOCKS section for more information on the lock information character.

```

仅此记录。

文章转载自 MySQL 学习，如果涉嫌侵权，请发送邮件至：contact@modb.pro 进行举报，并提供相关证据，一经查实，墨天轮将立刻删除相关内容。