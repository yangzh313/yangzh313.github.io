title:: [ERROR] InnoDB: Operating system error number 13 in a file operation

- ## solution
- 临时关闭selinux
	- `setenforce 0`
- 永久关闭
	- 修改`/etc/selinux/config`，SELINUX值修改为disabled
-
- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/heizistudio/article/details/45562881)
  
  在 Red Hat 中启动 MySQL：
  
  [root@local~]#service mysqld start
  
  Starting mysqld daemon [FAILED]
  
  2. 先看错误日志
  
  采用 rpm 安装的 MySQL 日志文件的默认路径是 / var/log/mysqld.log
  
  mysqld started
  
  InnoDB: Operating system error number 13 in a file operation.
  
  InnoDB: The error means mysqld does not have the access rights to
  
  InnoDB: the directory.
  
  InnoDB: File name /var/lib/mysql/ibdata1
  
  InnoDB: File operation call: 'create'.
  
  InnoDB: Cannot continue operation.
  
  mysqld ended
  
  从日志可以知道，mysql 的数据文件目录没有权限，所以我需要按照 master 上的目录权限给了：/var/lib/mysql。
  
  接着我又启动了 MySQL，但是还是失败，日志信息还是一样。
  
  3. 修改配置文件 / etc/selinux/config
  
  最后，我在外国友人的邮件来往中查到这样的回复：
  
  If you are using SE linux, set it to permissive mode by editing /etc/selinux/config
  
  and changing
  
  **SELINUX=enforcing to SELINUX=permissive**
  
  This solved all of my problems with the
  
  "Operating system error number 13 in a file operation" error
  
  所以，最后确定为 selinux 的问题，输入命令 ls -Z，你会发现在文件或文件夹上面打的标签。如果一个文件是在 selinux 打开的时候创建的，那么即使你关闭 selinux，它的权限控制还是会起作用的。可以通过 chcon 来改变权限。
  
  4. MySQL 成功启动
  
  [root@local~]#service mysqld start
  
  Starting mysqld daemon [OK]
  
  SELinux 之一：SELinux 基本概念及基本配置  
  
  SELinux 从出现至今，已经走过将近 13 年历史，然而在 Linux 相关 QQ 技术群或者 Linux 相关论坛，经常有人遇到问题问题都归咎与 SELinux，如 httpd 各项配置都正常，但客户就是无法访问；又比如 vsftpd 配置均正常，但客户端访问提示无法转换家目录。于是乎很多人都 对 SELinux 有了极大的偏见，认为 SELinux 带来的似乎只有麻烦，于是很多人选择在安装系统第一件事就是将 SELinux 设置为禁用。不过随着日 益增长的 0-day 安全漏洞，SELinux 在很多时候给了我们关键的保障。
  
  什么是 SELinux？
  ------------
  
  SELinux 全称 Security Enhanced Linux (安全强化 Linux), 是美国国家安全局 2000 年以 GNU GPL 发布，是 MAC (Mandatory Access Control，强制访问控制系统)的一个实现，目的在于明确的指明某个进程可以访问哪些资源 (文件、网络端口等)。强制访问控制系统 的用途在于增强系统抵御 0-Day 攻击(利用尚未公开的漏洞实现的攻击行为) 的能力。所以它不是网络防火墙或 ACL 的替代品，在用途上也 不重复。在目前的大多数发行版中，已经默认在内核集成了 SELinux。
  
  举例来说，系统上的 Apache 被发现存在一个漏洞，使得某远程用户可以访问系统上的敏感文件 (比如 /etc/passwd 来获得系统已存在用户) ，而修复该安全漏洞的 Apache 更新补丁尚未释出。此时 SELinux 可以起到弥补该漏洞的缓和方案。因为 /etc/passwd 不具有 Apache 的 访问标签，所以 Apache 对于 /etc/passwd 的访问会被 SELinux 阻止。 相比其他强制性访问控制系统，SELinux 有如下优势：
  
  *   控制策略是可查询而非程序不可见的。
  *   可以热更改策略而无需重启或者停止服务。
  *   可以从进程初始化、继承和程序执行三个方面通过策略进行控制。
  *   控制范围覆盖文件系统、目录、文件、文件启动描述符、端口、消息接口和网络接口。
  
  那么 SELinux 对于系统性能有什么样的影响呢? 根据 Phoronix 在 2009 年使用 Fedora 11 所做的横向比较来看，开启 SELinux 仅在少数 情况下导致系统性能约 5% 的降低。
  
  SELinux 是不是会十分影响一般桌面应用及程序开发呢？原因是，因为 SELinux 的策略主要针对服务器环境。但随着 SELinux 13 年来的广泛 应用，目前 SELinux 策略在一般桌面及程序开发环境下依然可以同时满足安全性及便利性的要求。以刚刚发布的 Fedora 15 为例，笔者在 搭建完整的娱乐 (包含多款第三方原生 Linux 游戏及 Wine 游戏) 及开发环境 (Android SDK + Eclipse) 过程中，只有 Wine 程序的首次运行 时受到 SELinux 默认策略的阻拦，在图形化的 “SELinux 故障排除程序” 帮助下，点击一下按钮就解决了。
  
  了解和配置 SELinux
  -------------
### 1. 获取当前 SELinux 运行状态

> getenforce

可能返回结果有三种：Enforcing、Permissive 和 Disabled。Disabled 代表 SELinux 被禁用，Permissive 代表仅记录安全警告但不阻止 可疑行为，Enforcing 代表记录警告且阻止可疑行为。

目前常见发行版中，RHEL、CentOS、Fedora 等默认设置为 Enforcing，其余的如 openSUSE 等为 Permissive。
### 2. 改变 SELinux 运行状态

> setenforce [Enforcing | Permissive | 1 | 0]

该命令可以立刻改变 SELinux 运行状态，在 Enforcing 和 Permissive 之间切换，结果保持至关机。一个典型的用途是看看到底是不是 SELinux 导致某个服务或者程序无法运行。若是在 setenforce 0 之后服务或者程序依然无法运行，那么就可以肯定不是 SELinux 导致的。

若是想要永久变更系统 SELinux 运行环境，可以通过更改配置文件 /etc/selinux/config 实现。注意当从 Disabled 切换到 Permissive 或者 Enforcing 模式后需要重启计算机并为整个文件系统重新创建安全标签 (touch /.autorelabel && reboot)。

> [root@web2 ~]# vim /etc/selinux/config
> 
> # This file controls the state of SELinux on the system.  
> # SELINUX= can take one of these three values:  
> # enforcing - SELinux security policy is enforced.  
> # permissive - SELinux prints warnings instead of enforcing.  
> # disabled - No SELinux policy is loaded.  
> **SELINUX=enforcing**  
> # SELINUXTYPE= can take one of these two values:  
> # targeted - Targeted processes are protected,  
> # mls - Multi Level Security protection.  
> SELINUXTYPE=targeted

3. SELinux 运行策略
---------------

配置文件 /etc/selinux/config 还包含了 SELinux 运行策略的信息，通过改变变量 SELINUXTYPE 的值实现，该值有两种可能： targeted 代表仅针对预制的几种网络服务和访问请求使用 SELinux 保护，strict 代表所有网络服务和访问请求都要经过 SELinux。

RHEL、CentOS、Fedora 等默认设置为 targeted，包含了对几乎所有常见网络服务的 SELinux 策略配置，已经默认安装并且可以无需修改直接使用。 若是想自己编辑 SELinux 策略，也提供了命令行下的策略编辑器 seedit 以及 Eclipse 下的编辑插件 eclipse-slide 。
### 4. coreutils 工具的 SELinux 模式

常见的属于 coreutils 的工具如 ps、ls 等等，可以通过增加 Z 选项的方式获知 SELinux 方面的信息。
#### 4.1 使用 ps 获取：

> [barlow@web1 ~]$ ps -auxZ |grep httpd |head -5
> 
> Warning: bad syntax, perhaps a bogus '-'? See /usr/share/doc/procps-3.2.8/FAQ unconfined_u:system_r:httpd_t:s0 apache 1393 0.0 0.2 279648 2272 ? S Jun27 0:01 /usr/sbin/httpd unconfined_u:system_r:httpd_t:s0 apache 1395 0.0 1.5 272964 15528 ? S Jun27 0:02 /usr/sbin/httpd unconfined_u:system_r:httpd_t:s0 apache 1399 0.0 1.7 272964 17828 ? S Jun27 0:02 /usr/sbin/httpd unconfined_u:system_r:httpd_t:s0 apache 1405 0.0 1.2 272964 12732 ? S Jun27 0:02 /usr/sbin/httpd unconfined_u:system_r:httpd_t:s0 apache 1409 0.0 1.4 272968 14784 ? S Jun27 0:03 /usr/sbin/httpd
#### 4.2 使用 ls 获取

> [barlow@web1 ~]$ ls -Z /var/www/ drwxrwxrwx. apache barlow unconfined_u:object_r:httpd_sys_script_exec_t:s0 cgi-bin drwxrwxrwx. apache barlow unconfined_u:object_r:httpd_sys_content_t:s0 error drwxrwxrwx. apache barlow unconfined_u:object_r:httpd_sys_content_t:s0 html drwxrwxrwx. apache barlow unconfined_u:object_r:httpd_sys_content_t:s0 icons drwxrwxrwx. apache barlow system_u:object_r:httpd_sys_content_t:s0 lost+found

以此类推，Z 选项可以应用在几乎全部 coreutils 工具里。
### 5、常用修改有关 httpd 服务的 SELinux 策略方法：

如上，ls -Z 方法查询到的文件 SELinux 上下文跟默认要求的不匹配，则服务无法正常使用，如 SELinux 要求 httpd 服务的网页目录或文件的上 下文要为 httpd_sys_content_t，否则客户端无法访问。
#### 5.1 使用 chcon 修改 httpd 目录或文件安全上下文：

如 nagios 服务器的网页目录上下文默认为 unconfined_u:object_r:usr_t:s0，则客户端无法访问：

> [root@nagios ~]# ll -Z /usr/local/nagios/share/
> 
> -rwxrwxr-x. nagios apache system_u:object_r:usr_t:s0 config.inc.php  
> drwxrwxr-x. nagios apache unconfined_u:object_r:usr_t:s0 contexthelp  
> drwxrwxr-x. nagios apache unconfined_u:object_r:usr_t:s0 docs  
> drwxrwxr-x. nagios apache unconfined_u:object_r:usr_t:s0 images  
> drwxrwxr-x. nagios apache unconfined_u:object_r:usr_t:s0 includes  
> -rwxrwxr-x. nagios apache system_u:object_r:usr_t:s0 index.html  
> -rwxrwxr-x. nagios apache system_u:object_r:usr_t:s0 index.php  
> …… 以下略……

使用 chcon 修改 / usr/local/nagios/share / 目录及其下所有文件安全上下文为 unconfined_u:object_r:httpd_sys_content_t

> [root@nagios ~]# chcon -R unconfined_u:object_r:httpd_sys_content_t:s0 /usr/local/nagios/share/

查询结果：

> [root@nagios ~]# ls -Z /usr/local/nagios/share/  
> -rwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 config.inc.php  
> drwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 contexthelp  
> drwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 docs  
> drwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 images  
> drwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 includes  
> -rwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 index.html  
> -rwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 index.php  
> drwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 locale  
> -rwxrwxr-x. nagios apache unconfined_u:object_r:httpd_sys_content_t:s0 main.html

不用重启 httpd 服务，客户端就已经可以访问。
#### 5.2 使用 semanage 工具，让 httpd 支持非标准端口：

semanage 工具非常强大，基本能实现所有 SELinux 配置，但很多时候我们并不知道 SELinux 错在哪里，在图形界面下有图形化的分析工具，在 终端界面下也有一个功能非常强大的分析工具 sealert，但默认情况下，这两个工具都没有被安装，需要先安装 semanage 和 sealert 工具：

> [root@web2 ~]# yum -y install policycoreutils-python setroubleshoot

注：semanage 的使用也可以参见我的另外一篇博文:[Selinux 管理工具 semanage](http://www.toxingwang.com/linux-unix/linux-basic/722.html)。

默认情况下 Apache 只侦听 80、443 等几个端口，若是直接指定其侦听 808 端口的话，会在 service httpd restart 的时候报错：

> [root@web2 ~]# service httpd start  
> 正在启动 httpd：(13)Permission denied: make_sock: could not bind to address 0.0.0.0:808  
> no listening sockets available, shutting down  
> Unable to open logs [失败]

查看 / var/log/messages 文件，可以看到如下这样的错误：

> [root@web2 ~]# tail /var/log/messages  
> Jun 29 10:30:51 web2 setroubleshoot: SELinux is preventing /usr/sbin/httpd from name_bind access on the tcp_socket . For complete SELinux messages. run sealert -l 2ad073a4-7c64-4175-93ff-9d78f75133af

根据提示，运行 sealert -l 2ad073a4-7c64-4175-93ff-9d78f75133af，生成 SELinux 报告如下：

> [root@web2 ~]# sealert -l 2ad073a4-7c64-4175-93ff-9d78f75133af  
> # semanage port -a -t PORT_TYPE -p tcp 808  
> 其中 PORT_TYPE 是以下之一：ntop_port_t, http_cache_port_t, http_port_t, puppet_port_t, jboss_messaging_port_t, jboss_management_port_t。

根据提示，运行 semanage port -a -t PORT_TYPE -p tcp 808，此处需将 PORT_TYPE 替换为 http_port_t

[root@web2 ~]# semanage port -a -t http_port_t -p tcp 808

查询结果：

> [root@web2 ~]# semanage port -l|grep http  
> http_cache_port_t tcp 3128, 8080, 8118, 8123, 10001-10010  
> http_cache_port_t udp 3130  
> http_port_t tcp **808,** 80, 443, 488, 8008, 8009, 8443 ## 可以看到 808 端口已经加入  
> pegasus_http_port_t tcp 5988  
> pegasus_https_port_t tcp 5989

重启服务：

> [root@web2 ~]# service httpd start  
> 正在启动 httpd： [确定]
#### 5.3 修改 selinux 布尔值，允许创建私人网站

若是希望用户可以通过在 ~/www/ 放置文件的方式创建自己的个人网站的话，那么需要在 Apache 策略中允许该操作执行。使用：

> [root@web2 ~]# setsebool httpd_enable_homedirs 1

默认情况下 setsebool 的设置只保留到下一次重启之前，若是想永久生效的话，需要添加 -P 参数，比如：

> [root@web2 ~]# setsebool -P httpd_enable_homedirs 1

setsebool 是用来切换由布尔值控制的 SELinux 策略的，当前布尔值策略的状态可以通过 getsebool 来获知。 查看与 httpd 相关的布尔值：

> [root@web2 ~]# getsebool -a |grep http  
> allow_httpd_anon_write --> off  
> allow_httpd_mod_auth_ntlm_winbind --> off  
> allow_httpd_mod_auth_pam --> off  
> allow_httpd_sys_script_anon_write --> off  
> httpd_builtin_scripting --> on  
> httpd_can_check_spam --> off  
> httpd_can_network_connect --> off  
> httpd_can_network_connect_cobbler --> off  
> httpd_can_network_connect_db --> on  
> httpd_can_network_memcache --> off  
> httpd_can_network_relay --> off  
> httpd_can_sendmail --> off  
> httpd_dbus_avahi --> on  
> httpd_enable_cgi --> on  
> httpd_enable_ftp_server --> off  
> httpd_enable_homedirs --> on  
> httpd_execmem --> off  
> httpd_manage_ipa --> off  
> httpd_read_user_content --> off  
> httpd_run_stickshift --> off  
> httpd_setrlimit --> off  
> httpd_ssi_exec --> off  
> httpd_tmp_exec --> off  
> httpd_tty_comm --> on  
> httpd_unified --> on  
> httpd_use_cifs --> off  
> httpd_use_gpg --> off  
> httpd_use_nfs --> on  
> httpd_use_openstack --> off  
> httpd_verify_dns --> off  
> named_bind_http_port --> off

以上列举了 httpd 服务常见 selinux 配置，其他服务（如 vsftpd）也可以采用相同的办法处理。