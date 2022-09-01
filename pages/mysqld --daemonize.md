- start with --daemonize
- ```
  [root@yangzh24 3307]# mysqld --no-defaults --datadir=/mysql/mydata/3307 --daemonize --socket=/mysql/mydata/3307/mysql.sock --log-error=/var/log/mysqld-3307.log --pid-file=/mysql/mydata/3307/yangzh24.pid --port=3307 --user=root
  [root@yangzh24 3307]#
  [root@yangzh24 3307]# netstat -ntlp|grep 3307
  tcp6       0      0 :::3307                 :::*                    LISTEN      24502/mysqld
  [root@yangzh24 3307]#
  [root@yangzh24 3307]# ps -ef|grep 24502
  root     24502     1  0 16:11 ?        00:00:00 mysqld --no-defaults --datadir=/mysql/mydata/3307 --daemonize --socket=/mysql/mydata/3307/mysql.sock --log-error=/var/log/mysqld-3307.log --pid-file=/mysql/mydata/3307/yangzh24.pid --port=3307 --user=root
  ```
- start without --daemonize
- ```
  [root@yangzh24 ~]# ps -ef|grep 24555
  root     24555 21320  0 16:13 pts/0    00:00:00 mysqld --no-defaults --datadir=/mysql/mydata/3307 --socket=/mysql/mydata/3307/mysql.sock --log-error=/var/log/mysqld-3307.log --pid-file=/mysql/mydata/3307/yangzh24.pid --port=3307 --user=root
  root     24657 24604  0 16:13 pts/11   00:00:00 grep --color=auto 24555
  [root@yangzh24 ~]#
  [root@yangzh24 ~]# ps -ef|grep 21320
  root     21320 21297  0 Jul18 pts/0    00:00:00 -bash
  root     24555 21320  0 16:13 pts/0    00:00:00 mysqld --no-defaults --datadir=/mysql/mydata/3307 --socket=/mysql/mydata/3307/mysql.sock --log-error=/var/log/mysqld-3307.log --pid-file=/mysql/mydata/3307/yangzh24.pid --port=3307 --user=root
  ```