# 7 Transaction

[MySQL 事务的隔离级别](https://developer.aliyun.com/article/743691)

InnoDB存储引擎中得事务完全符合ACID特性：

* 原子性（atomicity）
* 一致性（consistency）
* 隔离性（isolation）
* 持久性（durability）

## 7.1 认识事务

### 7.1.1 概述

事务时访问并更新数据库中各种数据项得一个程序执行单元。在事务中得操作，要么都做修改，要么都不做，这就是事务得目的。

**A（atomicity），原子性**。原子性指整个数据库事务是不可分割得工作单位。只有使事务中所有的数据库操作都执行成功，才算整个事务成功。事务中的任何SQL语句执行失败，必须回退到执行事务前的状态。
例如，用户从ATM机上取钱，不能用户钱未取到，但是钱已经扣了。

**C（consistency），一致性**。一致性指事务将数据库从一种状态转变为下一种一致的状态。在事务开始和事务结束以后，数据库的完整性约束没有被破坏。
例如，在表中有一个字段为姓名，为唯一约束，如果一个事务对其进行修改，但是在事务提交或发生回滚后，表中的姓名变得非唯一，则破坏了一致性。

**I（isolation），隔离性**。隔离性要求每个读写事务的对象对其他事务的操作对象能相互分离，即该事务提交前对其他事务都不可见，通常用锁实现。

**D（durability），持久性**。事务一旦提交，其结果就是永久性的。即使发生宕机等故障，数据库也能将数据恢复。持久性保证事务系统的高可靠性，而不是高可用性。

### 7.1.2 分类

从事务理论的角度来说，可以把事务分为以下几种类型：

* 扁平事务（Flat Transactions）
* 带有保存点的扁平事务（Flat Transactions with Savepoints）
* 链事务（Chained Transactions）
* 嵌套事务（Nested Transactions）
* 分布式事务（Distributed Transactions）

**扁平事务**是事务类型中最简单的一种，但在实际生产环境中，可能是使用最为频繁的事务。在扁平事务中，所有操作都处于同一层次，有BEGIN WORK开始，COMMIT WORK或ROLLBACK WORK结束，期间的操作是原子的，要么都执行，要么都回滚。

**带有保存点的扁平事务**，除了支持扁平事务支持的操作外，允许在事务执行过程中回滚到同一事务中较早的一个状态。**保存点**用来通知系统应该记住事务当前的状态，以前发生错误，事务能回到保存点当时的状态。

**链事务**可视为保存点模式的一种变种。带有保存点的扁平事务，保存点是易失的（volatile），而非持久的。意味着恢复时，事务需要从开始处重新执行。
链事务的思想是：在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务。提交事务操作和开始下一个事务操作合并为一个原子操作。
链事务中的回滚仅限于当前事务，即只能恢复到最近一个的保存点。对于锁的处理，锁事务在执行COMMIT后即释放了当前事务所持有的锁。

**嵌套事务**是一个层次结构框架。由一个顶层事务（top-level transaction）控制着各个层次的事务。

**分布式事务**通常是在一个分布式环境下运行的扁平事务，因此需要根据数据所在位置访问网络中的不同节点。
例如用户在ATM机（A）从招商银行（B）转账到工商银行（C），A发出转账，B减去10000，C加上10000，A通知用户操作完成或者操作失败。A不能通过调用一台数据库就完成任务，每个节点的数据库执行的事务操作都是扁平的。对于分布式事务，同样需要满足ACID特性，要么都发生，要么都失效。

InnoDB对于嵌套事务，并不原生支持。

## 7.2 事务的实现

事务隔离性由锁来实现。redo log称为重做日志，用来保证事务的原子性和持久性。undo log用来保证事务的一致性。
redo和undo的作用都可以视为一种恢复操作，redo恢复提交事务修改的页操作，undo回滚行记录到某个特定版本。redo通常是物理日志，undo是逻辑日志。

### 7.2.1 redo

**1. 基本概念**
redo log由两部分组成：一是内存中的重做日志缓冲（redo log buffer），其实易失的；二十重做日志文件（redo log file），其实持久的。
InnoDB通过Force Log at Commit机制实现事务的持久性。即当事务提交时，必须先将该事务的所有日志写入到重做日志文件进行持久化，待事务的COMMIT操作完成才算完成。
为了确保每次日志都写入重做日志文件，在每次将重做日志缓冲写入重做日志文件后，InnoDB存储引擎都需要调用一次fsync操作。由于重做日志打开并没有使用O_DIRECT选项，因此重做日志缓冲先写入文件系统缓存。为了确保重做日志写入磁盘，必须进行一次fsync操作。fsync的效率取决于磁盘的性能，因此磁盘的性能决定了事务提交的性能，也就是数据库的性能。
参数innodb_flush_log_at_trx_commit用来控制重做日志刷新到磁盘的策略。默认值为1，表示事务提交时必须调用一次fsync。0表示事务提交时不进行写入重做日志操作，这个操作仅在master thread中完成，即每1秒进行一次fsync。2表示事务提交时将重做日志写入重做日志文件，仅写入文件系统缓存，当数据库宕机二操作系统不发生宕机，事务不会丢失。
设置innodb_flush_log_at_trx_commit为0或2可以提高事务提交的性能，但丧失了事务的ACID特性。为了提高性能，多次操作可合并到一次COMMIT中来进行。
binlog与redo log不同的是：

* 重做日志是在InnoDB存储引擎曾产生，binlog则是在MySQL上层产生，任何存储引擎对于数据库的更改都会产生binlog
* 记录的内容形式不同。binlog是一种逻辑日志，记录SQL语句，redo log是物理日志，记录的是对于每个页的修改。
* 写入磁盘的时间点不同，binlog只在事务提交完成后进行一次写入，且仅包含对应事务地一个日志。而重做日志在事务进行中不断地被写入，每个事务对应多个日志条目，并且写入时并发，所以在文件中记录并非时事务开始地顺序。
**2. log block**
在InnoDB存储引擎中，重做日志都是以512字节进行存储地。意味着重做日志缓存、重做日志文件都是以块（block）地方式进行保存。称之为重做日志块。
由于重做日志块的大小和磁盘扇区大小一样都是512字节，因此重做日志的写入可以保证原子性。
重做日志块除了日志本身之外，还有日志块头（log block header）及日志块尾（log block tailer）两部分组成。头占用12字节，尾占用8字节，故每个块可以存储的大小为492字节。
**3. log group**
重做日志组，有多个重做日志文件。log group是一个逻辑上的概念，并没有实际的物理文件。InnoDB1.2版本开始重做日志文件总大小的限制提高到了512GB.
在InnoDB存储引擎运行过程中，log buffer根据一定的规则将内存中的log block刷新到磁盘。规则具体是：

* 事务提交时
* 当log buffer中有一半的内存空间已经被使用时
* log checkpoint时

**4. 重做日志格式**
不同的数据库操作会有对应的重做日志格式。由于InnoDB存储引擎的存储管理是基于页的，故其重做日志格式也是基于页的。虽然有着不同的重做日志格式，但有着通用的头部格式，由3部分组成：

* redo_log_type: 重做日志的类型。
* space: 表空间的ID。
* page_no：页的偏移量。

**5. LSN**
LSN是Log Sequence Number的缩写，代表日志序列号。在InnoDB中，LSN占用8字节，并且单调递增。含义由：

* 重做日志写入的总量
* checkpoint的位置
* 页的版本

**6. 恢复**
InnoDB存储引擎在启动时不管上次数据库运行时是否正常关闭，都会尝试进行恢复操作。因为重做日志记录的是物理日志，因此恢复的速度比逻辑日志，如二进制日志要快很多。
由于checkpoint表示已经刷新到磁盘页上的LSN，因此在恢复过程中仅需恢复checkpoint开始的日志部分。

### 7.2.2 undo

**1. 基本概念**
重做日志记录了事务的行为，可以很好地通过其对页进行“重做”操作。但是事务有时还需要进行回滚操作，这时就需要undo，可以利用undo信息将数据回滚到修改之前的样子。
undo存放在数据库内部的一个特殊段（segment）中，称为undo segment。undo段位于共享表空间内。
undo是逻辑日志，只是将数据库逻辑地恢复到原来的样子，而不是物理地恢复到之前，因为可能有其他事务也修改了相同地页。
除了回滚，undo地另一个作用是MVCC。当用户读取到一行记录时，若该记录已被其他事务占用，当前事务可以通过undo读取之前的行版本信息，以此实现非锁定读。
undo log会产生redo log，undo log也需要持久性的保护。
**2. undo存储管理**
InnoDB存储引擎对undo的管理同样采用段的方式。
事务提交后并不能马上删除undo log及undo log所在的页。可能还有其他事务需要通过undo log来得到行记录之前的版本。故事务提交时将undo log放入一个链表中，是否可以最终删除undo log及undo log所在的页由purge线程判断。
**3. undo log格式**
在innoDB存储引擎中，undo log分为：

* insert undo log
* update undo log

### 7.2.3 purge

delte和update操作可能并不直接删除原有的数据。例如对表t执行如下SQL语句：
DELETE FROM t WHERE a=1;
表t上列a有聚集索引，列b上有辅助索引。对于delete操作，仅是将主键列等于1的记录delete flag设置为1，记录还是存在B+树中。其次，辅助索引上a等于1，b等于1的记录同样没有做人任何处理。最终延时在purge操作中完成。

### 7.2.4 group commit

为提高磁盘fsync的效率，当前数据库都提供了group commit的功能，即一次fsync可以刷新多个事务日志被写入文件。
对于InnoDB存储引擎来说，事务提交时会进行两个阶段的操作：
1）修改内存中事务对应的信息，并且将日志写入重做日志缓冲。
2）调用fsync将确保日志都从重做日志缓冲写入磁盘。
步骤2相对步骤1是一个较慢的过程。但当有事务进行这个过程时，其他事务可以进行步骤1的操作，正在提交的事务完成提交操作后，再次进行步骤2时，可以将多个事务的重做日志通过一次fsync刷新到磁盘。对于写入或更新较为频繁的操作，group commit的效果尤为明显。

## 7.3 事务控制语句

默认设置下，事务都是自动提交的。要显式地开启一个事务需使用命令BEGIN、START TRANSACTION，或者执行命令SET AUTOCOMMIT=0，禁用当前会话地自动提交。可以使用地事务控制语句：

* START TRANSACTION|BEGIN: 显式地开启一个事务。
* COMMIT：也可写为COMMIT WORK，提交事务，使得对数据库地所有修改成为永久性的。
* ROLLBACK：ROLLBACK WORK，回滚会结束用户的事务，并撤销正在进行的所有未提交的修改。
* SAVEPOINT identifier：SAVEPOINT允许在事务中创建一个保存点，可以有多个。
* RELEASE SAVEPOINT identifier：删除一个事务的保存点。
* ROLLBACK TO[SAVEPOINT]identifier：可以把事务回滚到标记点，而不会滚在此标记点之前的任何工作。
* SET TRANSACTION：设置事务的隔离级别。

## 7.4 隐式提交的SQL语句

以下SQL语句会产生一个隐式的提交操作。

* DDL语句：ALTER DATABASE...UPGRADE DATA DIRECTORY NAME, ALTER EVENT, ALTER PROCEDURE, ALTER TABLE, ALTER VIEW, CREATE DATABASE, CREATE EVENT, CREATE INDEX, CREATE PROCEDURE, CREATE TABEL, CREATE TRIGGER, CREATE VIEW, DROP DATABASE, DROP EVENT, DROP INDEX, DROP PROCEDURE, DROP TABLE, DROP TRGGER, DROP VIEW, RENAEM TABLE, TRUNCATE TABLE。
* 用来隐式地修改MySQL架构地操作：CREATE USER, DROP USER, GRANT, RENAME USER, REVOKE, SET PASSWORD。
* 管理语句：ANALYZE TABLE, CACHE INDEX, CHECK TABLE, LOAD INDEX INTO CACHE, OPTIMIZE TABLE, REPAIRE TABLE。

需要注意的是，TRUNCATE TABLE语句是DDL，因此虽然和对整张表执行DELETE的结果是一样的，但是不能被回滚。

## 7.5 对于事务操作的统计

由于InnoDB存储引擎是支持事务的，因此InnoDB的应用需要在考虑每秒请求数（Question Per Second, QPS）的同时，应该关注每秒事务处理的能力（Transaction Per Second，TPS）。
计算TPS的方法是（com_commit+com_rollback)/time。

```MySQL
mysql> show global status like 'com_commit'\G
*************************** 1. row ***************************
Variable_name: Com_commit
        Value: 1
1 row in set (0.00 sec)

mysql> show global status like 'com_rollback'\G
*************************** 1. row ***************************
Variable_name: Com_rollback
        Value: 0
1 row in set (0.00 sec)

```

## 7.6 事务的隔离级别

SQL标准定义的四个隔离级别：

* READ UNCOMMITTED
* READ COMMITTED
* REPEATABLE READ
* SERIALIZABLE

READ UNCOMMITTED称为浏览访问(browse access)，仅仅针对事务而言的READ COMMITTED称为游标稳定（cursor stability）。REPEATABLE READ是2.9999°的隔离，没有幻读的保护。SERIALIZABLE称为隔离，或3°的隔离。
InnoDB默认隔离级别是REPEATABLE READ，使用Next-Key Lock锁的算法，避免幻读的产生。

## 7.7 分布式事务

### 7.7.1 MySQL数据库分布式事务

InnoDB提供了对XA事务的支持，通过其来支持分布式事务的实现。分布式事务指的是允许多个独立的事务资源（transactional resources）参与到一个全局的事务中。全局事务要求在其中的所有参与的事务要么都提交，要么都回滚。在使用分布式事务时，InnoDB隔离级别必须设置为SERIALIZABLE。
XA事务允许不同数据库之间的分布式事务。
XA事务由一个或多个资源管理器、一个事务管理器以及一个应用程序组成。

* 资源管理器：提供访问事务资源的方法，通常一个数据库就是一个资源管理器。
* 事务管理器：协调参与全局事务中的各个事务。需要和参与全局事务的所有资源管理器进行通信。
* 应用程序：定义事务的边界，指定全局事务中的操作。

在MySQL数据库的分布式事务中，资源管理器就是MySQL数据库，事务管理器为连接MySQL服务器的客户端。
分布式事务使用两段式提交（two-phase commit）的方式。第一阶段，所有参与全局事务的节点都开始准备（PREPARE），告诉事务管理器它们准备好提交了。第二阶段，事务管理器告诉资源管理器执行ROLLBACK还是COMMIT。如果任何一个节点显示不能提交，则所有的节点都被告知需要回滚。
默认启用了XA事务的支持。

```MySQL
mysql> SHOW VARIABLES LIKE 'innodb_support_xa'\G';
*************************** 1. row ***************************
Variable_name: innodb_support_xa
        Value: ON
1 row in set (0.00 sec)
```

### 7.7.2 内部XA事务

之前讨论的分布式事务是外部事务，即资源管理器是MySQL数据库本身。在存储引擎与插件之间，又或者在存储引擎与存储引擎之间，还有另外一种内部XA事务。
由于复制的需要，大多数书库库开启了binlog功能，在事务提交时，先写二进制日志，再写重做日志，上述两个操作是原子的。
为了解决可能出现的主从不一致的情况，在binlog与InnoDB存储引擎之间采用XA事务。当事务提交时，InnoDB存储引擎会先做一个PREPARE操作，将事务的xid写入，接着进行二进制日志的写入，如果在InnoDB提交前，数据库宕机了，那么恢复时会先检查准备的UXID事务是否已经提交，若没有，则在存储引擎层再进行一次提交操作。

## 7.8 不好的事务习惯

### 7.8.1 在循环中提交

### 7.8.2 使用自动提交

可以使用SET autocommit=0;改变当前自动提交的方式。也可以使用START TRANSACTION, BEGIN来显式地开启一个事务。默认设置（completion_type等于0）,显示开启后，MySQL会在结束一个事务后执行SET autocommit=1;

### 7.8.3 使用自动回滚

InnoDB支持通过定义一个HANDLER来进行自动事务地回滚操作，如在一个存储过程中发生了错误会自动对其进行回滚操作。
对于开发人员来说，重要的不仅是知道发生了错误，而是发生了什么样的错误。
存储过程需要完成的只是一个逻辑的操作，对事物的BEGIN, COMMIT和ROLLBACK操作应该交给程序端来完成。

## 7.9 长事务

Long-Lived Transactions, 就是执行时间较长的事务。对于长事务的问题，优势可以通过转化为小批量的事务来进行处理。当事务发生错误时，只需要回滚一部分数据，然后接着上次已完成的事务继续进行。
