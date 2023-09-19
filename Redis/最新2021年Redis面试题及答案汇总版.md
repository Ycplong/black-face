# 最新2021年Redis面试题及答案汇总版

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[Redis 如何查看使用情况及状态信息？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题1redis-如何查看使用情况及状态信息)<br/>
使用info命令。

### 题2：[Redis 官方为什么不提供 Windows 版本？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题2redis-官方为什么不提供-windows-版本)<br/>
Redis官方不提供Window 版本主要是因为目前Linux版本已经相当稳定，而且用户量很大。

如果开发windows版本Redis还需要考虑兼容性等问题。

### 题3：[什么是 Redis 持久化？Redis 有哪几种持久化方式？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题3什么是-redis-持久化redis-有哪几种持久化方式)<br/>
Redis持久化是指把内存的数据写到磁盘中去，防止服务因宕机导致内存数据丢失。

Redis提供了两种持久化方式：RDB（默认）和AOF。

RDB是Redis DataBase缩写，功能核心函数rdbSave（生成RDB文件）和rdbLoad（从文件加载内存）两个函数。

AOF是Append-only file缩写，每当执行服务器（定时）任务或者函数时flushAppendOnlyFile函数都会被调用，这个函数执行以下两个工作。

**AOF写入与保存**

WRITE：根据条件，将aof_buf中的缓存写入到AOF文件

SAVE：根据条件，调用fsync或fdatasync函数，将AOF文件保存到磁盘中。

### 题4：[Redis key 如何设置过期时间和永久有效？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题4redis-key-如何设置过期时间和永久有效)<br/>
Redis key设置过期时间使用EXPIRE命令。

Redis key设置永久有效使用PERSIST命令。

### 题5：[Redis 回收使用的是什么算法？ ](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题5redis-回收使用的是什么算法-)<br/>
Redis回收使用的是LRU算法。

LRU是Least Recently Used的缩写，即最近最少使用页面置换算法，是为虚拟页式存储管理服务的。

### 题6：[Redis 是单线程的吗？为什么这么快？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题6redis-是单线程的吗为什么这么快)<br/>
Redis是单线程的，至于为什么这么快主要是因如下几方面：

1）Redis是纯内存数据库，一般都是简单的存取操作，线程占用的时间很多，时间的花费主要集中在IO上，所以读取速度快。

2）Redis使用的是非阻塞IO、IO多路复用，使用了单线程来轮询描述符，将数据库的开、关、读、写都转换成了事件，减少了线程切换时上下文的切换和竞争。

3）Redis采用了单线程的模型，保证了每个操作的原子性，也减少了线程的上下文切换和竞争。

4）Redis避免了多线程的锁的消耗。

5）Redis采用自己实现的事件分离器，效率比较高，内部采用非阻塞的执行方式，吞吐能力比较大

### 题7：[Redis 中如何解决 The TCP backlog setting of 511 cannot be enforced 告警问题？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题7redis-中如何解决-the-tcp-backlog-setting-of-511-cannot-be-enforced-告警问题)<br/>
**告警信息**

```shell
WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
```

翻译为：【警告：无法强制执行TCP backlog设置511，因为/proc/sys/net/core/somaxconn设置为较低的值128。】

**解决方法**

方式一：设置的值128较低，需要“echo 511 > /proc/sys/net/core/somaxconn”命令，注意此命令只是暂时生效，如果重启后就会失效。

方式二：永久解决Redis中The TCP backlog setting of 511 cannot be enforced告警问题，编辑/etc/sysctl.conf文件，添加net.core.somaxconn = 1024然后执行sysctl -p命令查看是否添加成功，之后重启Redis服务即可。

```shell
[root@mrwang redis-4.0.9]# vim /etc/sysctl.conf 
[root@mrwang redis-4.0.9]# sysctl -p            
vm.swappiness = 0
kernel.sysrq = 1
net.ipv4.neigh.default.gc_stale_time = 120
net.ipv4.conf.all.rp_filter = 0
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.default.arp_announce = 2
net.ipv4.conf.lo.arp_announce = 2
net.ipv4.conf.all.arp_announce = 2
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 1024
net.ipv4.tcp_synack_retries = 2
net.core.somaxconn = 1024
```

### 题8：[什么是缓存穿透？如何避免？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题8什么是缓存穿透如何避免)<br/>
缓存穿透是指一般的缓存系统，都是按照key去缓存查询，如果不存在对应的value，就应该去后端系统查找（比如DB）。

一些恶意的请求会故意查询不存在的key，导致请求量大，造成后端系统压力大，这就是做缓存穿透。

**如何避免？**

1）对查询结果为空的情况也进行缓存，缓存时间设置短一点或key对应的数据insert后清理缓存。

2）对一定不存在的key进行过滤，可以把所有可能存在的key放到一个大的Bitmap中，查询时通过bitmap过滤。

### 题9：[Redis 事务能否保证原子性，是否支持回滚吗？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题9redis-事务能否保证原子性是否支持回滚吗)<br/>
Redis中单条命令是原子性执行的，但事务不能保证原子性，且没有回滚。

事务中任意命令执行失败，其余的命令仍会被执行。

### 题10：[Jedis 和 Redisson 有什么优缺点？](/docs/Redis/最新2021年Redis面试题及答案汇总版.md#题10jedis-和-redisson-有什么优缺点)<br/>
Jedis是Redis的Java实现的客户端，其API提供了比较全面的Redis命令的支持

Redisson实现了分布式和可扩展的Java数据结构，和Jedis相比，功能较为简单，不支持字符串操作，不支持排序、事务、管道、分区等Redis特性。

Redisson的宗旨是促进使用者对Redis的关注分离，从而让使用者能够将精力更集中地放在处理业务逻辑上。

### 题11：为什么-redis-集群的最大槽数是-16384-个<br/>


### 题12：支持一致性哈希的客户端有哪些<br/>


### 题13：为什么要用-redis-而不用-mapguava-做缓存<br/>


### 题14：redis-中如何实现分布式锁<br/>


### 题15：redis-集群会产生数据丢失情况吗<br/>


### 题16：redis-和-memcached-都有哪些区别<br/>


### 题17：redis-如何测试连通性<br/>


### 题18：redis-集群如何选择数据库<br/>


### 题19：redis-中如何解决-overcommit_memory-is-set-to-0-告警问题<br/>


### 题20：redis-支持那些数据类型<br/>


### 题21：redis-哈希槽的概念是什么<br/>


### 题22：redis-集群之间是如何复制的<br/>


### 题23：redis-将内存占满后会发生什么问题<br/>


### 题24：redis-如何设置密码及验证密码<br/>


### 题25：什么是-redis<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")