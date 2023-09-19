# 最新MySQL面试题及答案附答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## MySQL

### 题1：[MySQL 中 InnoDB 引擎的行锁是怎么实现的？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题1mysql-中-innodb-引擎的行锁是怎么实现的)<br/>
基于索引来完成行锁的。
```sql
select * from user where id = 766 for update;
```
for update可以根据条件来完成行锁锁定，并且id是有索引键的列，如果id不是索引键那么InnoDB将实行表锁。

### 题2：[SQL 注入漏洞是什么原因造成的？如何防止？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题2sql-注入漏洞是什么原因造成的如何防止)<br/>
造成SQL注入的原因是程序开发过程中不注意规范书写sql语句和对特殊字符进行过滤，导致客户端可以通过全局变量POST和GET提交一些sql语句正常执行。

防止SQL注入的方式：

1、开启配置文件中的magic_quotes_gpc 和 magic_quotes_runtime设置。

2、执行sql语句时使用addslashes进行sql语句转换。

3、Sql语句书写尽量不要省略双引号和单引号。

4、过滤掉sql语句中的一些关键词：update、insert、delete、select、*。

5、提高数据库表和字段的命名技巧，对一些重要的字段根据程序的特点命名，取不易被猜到的。

### 题3：[Mysql 中 MyISAM 和 InnoDB 的区别有哪些？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题3mysql-中-myisam-和-innodb-的区别有哪些)<br/>
1、InnoDB支持事务，MyISAM不支持

对于InnoDB每一条SQL语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条SQL语言放在begin和commit之间，组成一个事务；

2、InnoDB支持外键，而MyISAM不支持。对一个包含外键的InnoDB表转为MYISAM会失败；

3、InnoDB是聚集索引，数据文件是和索引绑在一起的，必须要有主键，通过主键索引效率很高。

但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此主键不应该过大，因为主键太大，其他索引也都会很大。而MyISAM是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。

4、InnoDB不保存表的具体行数，执行select count(*) from table时需要全表扫描。而MyISAM用一个变量保存了整个表的行数，执行上述语句时只需要读出该变量即可，速度很快；

5、Innodb不支持全文索引，而MyISAM支持全文索引，查询效率上MyISAM要高。

### 题4：[在高并发情况下，如何做到安全的修改同一行数据？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题4在高并发情况下如何做到安全的修改同一行数据)<br/>
要安全的修改同一行数据，就要保证一个线程在修改时其它线程无法更新这行记录。一般有悲观锁和乐观锁两种方案。

**使用悲观锁**

悲观锁思想是当前线程要进来修改数据时，别的线程都得拒之门外，比如可以使用如下SQL语句。
```java
select * from jingxuan where id=736 for update
```
以上这条sql语句会锁定了User表中所有符合检索条件（id=736）的记录。本次事务提交之前，别的线程都无法修改这些记录。

**使用乐观锁**

乐观锁思想是有线程过来，先放过去修改，如果看到别的线程没修改过，就可以修改成功，如果别的线程修改过，就修改失败或者重试。

实现方式：乐观锁一般会使用版本号机制或CAS算法实现。

### 题5：[MySQL 中都有哪些触发器？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题5mysql-中都有哪些触发器)<br/>
MySQL数据库中有六种触发器：

Before Insert

After Insert

Before Update

After Update

Before Delete

After Delete

### 题6：[MySQL 中如何获取当前日期？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题6mysql-中如何获取当前日期)<br/>
```sql
SELECT CURRENT_DATE();
```

### 题7：[Mysql 中使用 MyISAM 和 InnoDB 如何选择？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题7mysql-中使用-myisam-和-innodb-如何选择)<br/>
1、是否要支持事务，如果要请选择innodb，如果不需要可以考虑MyISAM；

2、如果表中绝大多数都只是读查询，可以考虑MyISAM，如果既有读写也挺频繁，请使用InnoDB

3、系统奔溃后，MyISAM恢复起来更困难，能否接受；

4、MySQL5.5版本开始Innodb已经成为Mysql的默认引擎(之前是MyISAM)，说明其优势是有目共睹的，如果你不知道用什么，那就用InnoDB，至少不会差。

### 题8：[组合索引是什么？为什么需要注意组合索引中的顺序？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题8组合索引是什么为什么需要注意组合索引中的顺序)<br/>
组合索引是指用户可以在多个列上建立索引，这种索引叫做组合索引。

因为InnoDB引擎中的索引策略的最左原则，所以需要注意组合索引中的顺序。

### 题9：[如何监控数据库？慢日志是如何查询的？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题9如何监控数据库慢日志是如何查询的)<br/>
监控数据库的工具有很多，例如zabbix、lepus等，比较常用的是zabbix。

### 题10：[时间戳如何在 Unix 和 MySQL 之间进行转换？](/docs/MySQL/最新MySQL面试题及答案附答案汇总.md#题10时间戳如何在-unix-和-mysql-之间进行转换)<br/>
UNIX_TIMESTAMP是从Mysql时间戳转换为Unix时间戳的命令。

FROM_UNIXTIME是从Unix时间戳转换为Mysql时间戳的命令。

### 题11：mysql-中-varchar(50)-中-50-是什么涵义<br/>


### 题12：mysql-中单表记录数过大时如何优化<br/>


### 题13：什么是表分区<br/>


### 题14：mysql-中行级锁都有哪些优缺点<br/>


### 题15：mysql-中自增主键可能会遇到什么问题<br/>


### 题16：mysql-中如何找出最后一次插入时分配的自动增量<br/>


### 题17：mysql-中如何去掉重复数据记录<br/>


### 题18：视图常见使用场景有哪些<br/>


### 题19：mysql-中-datetime-和-timestamp-有什么区别<br/>


### 题20：数据库表创建都有哪些注意事项<br/>


### 题21：mysql支持哪些分区类型<br/>


### 题22：mysql-事务都有哪些特性<br/>


### 题23：sql-语句中使用--like-如何避免索引失效<br/>


### 题24：什么是视图<br/>


### 题25：sql-语句执行过久如何优化从哪些方面入手<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")