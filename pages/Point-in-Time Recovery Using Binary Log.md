- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [dev.mysql.com](https://dev.mysql.com/doc/refman/5.7/en/point-in-time-recovery-binlog.html)
  
  > This section explains the general idea of using the binary log to perform a point-in-time-rec......
### 7.5.1 Point-in-Time Recovery Using Binary Log

This section explains the general idea of using the binary log to perform a point-in-time-recovery. The next section, [Section 7.5.2, “Point-in-Time Recovery Using Event Positions”](https://dev.mysql.com/doc/refman/5.7/en/point-in-time-recovery-positions.html "7.5.2 Point-in-Time Recovery Using Event Positions"), explains the operation in details with an example.

Note

Many of the examples in this and the next section use the [**mysql**](https://dev.mysql.com/doc/refman/5.7/en/mysql.html "4.5.1 mysql — The MySQL Command-Line Client") client to process binary log output produced by [**mysqlbinlog**](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files"). If your binary log contains `\0` (null) characters, that output cannot be parsed by [**mysql**](https://dev.mysql.com/doc/refman/5.7/en/mysql.html "4.5.1 mysql — The MySQL Command-Line Client") unless you invoke it with the [`--binary-mode`](https://dev.mysql.com/doc/refman/5.7/en/mysql-command-options.html#option_mysql_binary-mode) option.

The source of information for point-in-time recovery is the set of binary log files generated subsequent to the full backup operation. Therefore, to allow a server to be restored to a point-in-time, binary logging must be enabled on it (see [Section 5.4.4, “The Binary Log”](https://dev.mysql.com/doc/refman/5.7/en/binary-log.html "5.4.4 The Binary Log") for details).

To restore data from the binary log, you must know the name and location of the current binary log files. By default, the server creates binary log files in the data directory, but a path name can be specified with the [`--log-bin`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#option_mysqld_log-bin) option to place the files in a different location. To see a listing of all binary log files, use this statement:

To determine the name of the current binary log file, issue the following statement:

```
mysql> SHOW MASTER STATUS;

```

The [**mysqlbinlog**](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files") utility converts the events in the binary log files from binary format to text so that they can be viewed or applied. [**mysqlbinlog**](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files") has options for selecting sections of the binary log based on event times or position of events within the log. See [Section 4.6.7, “mysqlbinlog — Utility for Processing Binary Log Files”](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files").

Applying events from the binary log causes the data modifications they represent to be reexecuted. This enables recovery of data changes for a given span of time. To apply events from the binary log, process [**mysqlbinlog**](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files") output using the [**mysql**](https://dev.mysql.com/doc/refman/5.7/en/mysql.html "4.5.1 mysql — The MySQL Command-Line Client") client:

```
$> mysqlbinlog binlog_files | mysql -u root -p

```

Viewing log contents can be useful when you need to determine event times or positions to select partial log contents prior to executing events. To view events from the log, send [**mysqlbinlog**](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "4.6.7 mysqlbinlog — Utility for Processing Binary Log Files") output into a paging program:

```
$> mysqlbinlog binlog_files | more

```

Alternatively, save the output in a file and view the file in a text editor:

```
$> mysqlbinlog binlog_files > tmpfile
$> ... edit tmpfile ...

```

After editing the file, apply the contents as follows:

```
$> mysql -u root -p < tmpfile

```

If you have more than one binary log to apply on the MySQL server, use a single connection to apply the contents of all binary log files that you want to process. Here is one way to do so:

```
$> mysqlbinlog binlog.000001 binlog.000002 | mysql -u root -p

```

Another approach is to write the whole log to a single file and then process the file:

```
$> mysqlbinlog binlog.000001 >  /tmp/statements.sql
$> mysqlbinlog binlog.000002 >> /tmp/statements.sql
$> mysql -u root -p -e "source /tmp/statements.sql"

```