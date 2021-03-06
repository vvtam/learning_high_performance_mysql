# 第1章 MySQL架构与历史
## 1.1 MySQL逻辑架构

MySQL的存储引擎架构，查询处理，系统任务，数据存储、提取 分离   
MySQL逻辑架构图

![mysql服务器逻辑架构图](.\images\mysql服务器逻辑架构图.png)

第一层   
连接处理，授权认证，安全

第二层     
查询解析，分析，优化，缓存，内置函数   
跨存储引擎的功能在这一层实现 （存储过程，触发器，视图）

第三层  
存储引擎   
负责数据的存储和提取   

知识点   
读写锁  
共享锁，排他锁（读锁，写锁）  
读锁是共享的，相互不阻塞，写锁是排他的，一个写锁会阻塞其它的写锁和读锁  
锁粒度   
表锁（table lock），行级锁（row lock），行级锁只在存储引擎层实现   
锁粒度不同，会影响系统性能   
事务   
事务的ACID概念？atomicity consistency isolation durability，原子性 一致性，隔离性，持久性  
一组原子性的SQL查询，要么全部执行，要么全部失败（比如银行转账事务，查询余额，减去金额，增加金额）    
START TRANSACTION语句开始事务，COMMIT提交事务，ROLLBACK撤销说有修改   
atomicity（原子性），最小工作单元   
consistency（一致性），从一个一致性状态到另一个一致性状态，事务未提交，则修改不会保存  
isolation（隔离性），**通常来说**，事务未提交以前，对其它事务不可见      
durability（持久性），事务提交，则数据永久保存到数据库中   
根据应用来选择是否需要支持事务的存储引擎，可以获得更好的系统性能    

死锁？   
事务日志，修改表时修改内存拷贝，持久到硬盘上的事务日志，在写回存储空间（日志是磁盘上的一小块区域的I/O，而写到存储空间一般是随机的I/O）   
已经写了事务日志，但是没有写回磁盘，如果系统崩溃，存储引擎可以自动恢复这部分数据   
支持事务的存储引擎 InnoDB，NDB Cluster，XtraDB，PBXT等   
自动提交 autocommit，另外有一些命令，在执行之前会强制执行 commit 提交之前活动的事务，比如ALTER，LOCK TABLES

SET SESSION TRANSACTION ISOLATION LEVEL xx;来设置隔离级别

MVCC，多版本并发控制，保存某个时间点的快照来实现

存储引擎   
frm文件存储表的定义   
InnoDB的数据存储在表空间，表空间是InnoDB管理的一个黑盒子，是一系列的数据文件。      
转换表的引擎   
1.ALTER TABLE mytable ENGINE = InnoDB;     
2.导出，导入创建表的时候修改引擎;   
3.CREATE，然后INSERT...SELECT 数据量大了可以根据主键分批操作   Percona Toolkit 有工具 pt-online-schema-change    

### 1.1.1 连接管理和安全性

### 1.1.2 优化与执行

## 1.2 并发控制

### 1.2.1 读写锁

### 1.2.2 锁粒度

## 1.3 事务

```
START TRANSACTION;
SELECT XXX;
UPDATE XXX;
UPDATE XXX;
COMMIT; [ROLLBACK;]
```

ACID，原子性（atomicity），一致性（consistency），隔离性（isolation），持久性（durability）。###

### 1.3.1 隔离级别

### 1.3.2 死锁

### 1.3.3 事务日志

### 1.3.4 MySQL 中的事务

AUTOCOMMIT，或者ALTER TABLE，LOCK TABLES等语句触发自动提交

设置隔离级别

````
SET TRANSACTION ISOLATION LEVEL READ COMMITED;
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
在my.cnf配置
````