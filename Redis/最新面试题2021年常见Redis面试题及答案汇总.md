# 最新面试题2021年常见Redis面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[Redis 中如何解决 overcommit_memory is set to 0 告警问题？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题1redis-中如何解决-overcommit_memory-is-set-to-0-告警问题)<br/>
**告警信息**

```shell
WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
```

翻译为：警告内存设置为0！在内存不足的情况下，后台保存可能会失败。若要解决此问题，请将“vm.overcommit_memory = 1”添加到/etc/sysctl.conf，然后重新启动或运行命令“sysctl vm.overcommit_memory=1”使其生效。

**解决办法**

方式一：执行“echo 1 > /proc/sys/vm/overcommit_memory”命令，如果没有权限执行该命令需要切换至root账户，执行su root命令使用root用户。这会使其立即生效，不用重启redis服务，但是如果redis服务重启后还会报错误，需要再次执行上述命令。

方式二：彻底解决Redis中overcommit_memory is set to 0!告警问题，编辑/etc/sysctl.conf文件，添加net.core.somaxconn = 1024然后执行sysctl -p命令查看是否添加成功，之后重启Redis服务即可。

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
vm.overcommit_memory = 1
```

### 题2：[Redis 中如何解决 The TCP backlog setting of 511 cannot be enforced 告警问题？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题2redis-中如何解决-the-tcp-backlog-setting-of-511-cannot-be-enforced-告警问题)<br/>
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

### 题3：[Redis 过期键都有哪些删除策略？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题3redis-过期键都有哪些删除策略)<br/>
Redis是key-value数据库，可以设置Redis中缓存key的过期时间。

Redis的过期策略是指当Redis中缓存的key过期，Redis是如何处理。

常见的删除策略有以下三种：

**定时删除**

在设置key的过期时间的同时，创建一个定时器，让定时器到过期时间就立即执行对key的删除操作。

需要注意的是这种策略可以立即清除过期的数据，对内存很友好；但是会占用大量的CPU资源去处理过期的数据，从而影响缓存的响应时间和吞吐量。

**惰性删除**

只有当访问一个key时，才会判断这个key是否已过期，若果过期急就删除，反之没有过期，就返回该key。

需要注意的是这种策略可以最大化地节省CPU资源，但是对内存非常不友好。甚至极端情况可能出现大量的过期key，因没有再次被访问从而不会被清除，占用大量内存。

**定期删除**

每隔一段时间，程序对数据库进行一次检查，删除里面的过期键，至于要删除哪些数据库的哪些过期键，则由算法决定。

每隔一定的时间，程序会扫描一定数量的数据库的expires字典中一定数量的key并删除其中已过期的key。至于要删除哪些数据库的哪些过期key，则由算法决定。

需要注意的是这种策略是前两者的一个折中方案。通过调整定时扫描的时间间隔和每次扫描的限定耗时，可以在不同情况下使得CPU和内存资源达到最优的平衡效果。

>expires字典保存所有设置过期key的过期时间数据。
key是指向键空间中的某个键的指针，value是该键的毫秒精度的UNIX时间戳表示的过期时间。
键空间是指该Redis集群中保存的所有键。

其中定时删除和定期删除为主动删除策略，惰性删除为被动删除策略，Redis中同时使用了惰性删除和定期删除两种删除策略。

### 题4：[Redis 事务能否保证原子性，是否支持回滚吗？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题4redis-事务能否保证原子性是否支持回滚吗)<br/>
Redis中单条命令是原子性执行的，但事务不能保证原子性，且没有回滚。

事务中任意命令执行失败，其余的命令仍会被执行。

### 题5：[Redis 事务命令都有哪几个？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题5redis-事务命令都有哪几个)<br/>
MULTI、EXEC、DISCARD、WATCH

multi：标记一个事务块的开始，返回ok。

exec：执行所有事务块内，事务块内所有命令执行的先后顺序的返回值，操作被，返回空值nil。

discard：取消事务，放弃执行事务块内的所有命令，返回ok。

watch：监视key在事务执行之前是否被其他指令改动，若已修改则事务内的指令取消执行，返回ok。

unwatch：取消watch命令对key的监视，返回ok。

### 题6：[Redis 如何查看使用情况及状态信息？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题6redis-如何查看使用情况及状态信息)<br/>
使用info命令。

### 题7：[Redis 官方为什么不提供 Windows 版本？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题7redis-官方为什么不提供-windows-版本)<br/>
Redis官方不提供Window 版本主要是因为目前Linux版本已经相当稳定，而且用户量很大。

如果开发windows版本Redis还需要考虑兼容性等问题。

### 题8：[Redis key 如何设置过期时间和永久有效？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题8redis-key-如何设置过期时间和永久有效)<br/>
Redis key设置过期时间使用EXPIRE命令。

Redis key设置永久有效使用PERSIST命令。

### 题9：[Redis 和其他key-value存储有什么不同？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题9redis-和其他key-value存储有什么不同)<br/>
Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。

Redis数据类型都是基于基本数据结构，同时对开发人员透明，无需进行额外的抽象。

Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，应为数据量不能大于硬件内存。

内存数据库方面的优点是在内存比在磁盘上相同复杂的数据结构上操作更为简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面它们是紧凑并以追加的方式产生，因为它们并不需要进行随机访问。

### 题10：[Redis 是单线程的，如何提高多核 CPU 的利用率？](/docs/Redis/最新面试题2021年常见Redis面试题及答案汇总.md#题10redis-是单线程的如何提高多核-cpu-的利用率)<br/>
可以在同一个服务器部署多个Redis的实例，并把他们当作不同的服务器来使用，在某些时候，无论如何一个服务器是不够的， 所以，如果你想使用多个CPU，你可以考虑一下分片(shard)。

### 题11：为什么要用-redis-而不用-mapguava-做缓存<br/>


### 题12：什么是-redis-持久化redis-有哪几种持久化方式<br/>


### 题13：redis-集群之间是如何复制的<br/>


### 题14：什么是缓存雪崩何如避免<br/>


### 题15：redis-和-memcached-都有哪些区别<br/>


### 题16：redis-中如何解决-thp-服务导致的延迟和内存使用问题<br/>


### 题17：redis-是单线程的吗为什么这么快<br/>


### 题18：redis-如何设置密码及验证密码<br/>


### 题19：redis-中分布式锁有什么缺陷性问题<br/>


### 题20：redis-集群会产生数据丢失情况吗<br/>


### 题21：jedis-和-redisson-有什么优缺点<br/>


### 题22：什么是缓存穿透如何避免<br/>


### 题23：redis-持久化数据如何实现扩容<br/>


### 题24：redis-事务支持隔离性吗<br/>


### 题25：支持一致性哈希的客户端有哪些<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")