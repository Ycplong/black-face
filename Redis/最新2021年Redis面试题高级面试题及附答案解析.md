# 最新2021年Redis面试题高级面试题及附答案解析

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[为什么要用 Redis 而不用 Map、Guava 做缓存？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题1为什么要用-redis-而不用-mapguava-做缓存)<br/>
缓存可以划分为本地缓存和分布式缓存。

以Java为例，使用自带Map或者Guava类实现的是本地缓存，主要特点是轻量以及快速，它们的生命周期会随着JVM的销毁而结束，且在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。

使用Redis或Memcached等被称为分布式缓存，在多实例的情况下，各实例共用一份缓存数据，具有一致性，但是需要保持Redis或Memcached服务的高可用。

### 题2：[Redis 将内存占满后会发生什么问题？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题2redis-将内存占满后会发生什么问题)<br/>
如果达到设置的上限，Redis的写命令会返回错误信息，但是读命令还可以正常返回。

可以配置内存淘汰机制，当Redis达到内存上限时会冲刷掉旧的内容。

### 题3：[Redis 如何处理数据存储实现内存优化？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题3redis-如何处理数据存储实现内存优化)<br/>
尽量使用redis的散列表，把相关的信息放到散列表里面存储，而不是把每个字段单独存储，这样可以有效的减少内存使用。

可以通过使用Hash、list、sorted set、set等集合类型数据，因为通常情况下很多小的Key-Value可以用更紧凑的方式存放到一起。尽可能使用散列表（hashes），散列表（是说散列表里面存储的数少）使用的内存非常小，所以你应该尽可能的将你的数据模型抽象到一个散列表里面。

比如web系统中有一个用户对象，不要为这个用户的名称，姓氏，邮箱，密码设置单独的key，而是把这个用户的所有信息存储到一张散列表中。

### 题4：[Redis 都有哪些使用场景？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题4redis-都有哪些使用场景)<br/>
1、延迟操作

订单入库时数据量过于庞大，可以用于Redis缓存数据量，减轻数据库压力，再分批处理数据。

2、点赞、好友等相互关系

3、Session共享

使用Redis可以实现单点登录，通过expire控制是否失效问题。

4、分布式锁

5、计数器

6、排行榜

使用Redis中SortedSet类型进行数据的排序。

7、模糊搜索

数据量大时可以使用Redis缓存，进行模糊搜索。

8、队列

Redis支持list push和list pop命令，因此很方便的执行队列操作。

### 题5：[什么是 Redis 事务？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题5什么是-redis-事务)<br/>
Redis事务的本质是通过MULTI、EXEC、WATCH等一组命令的集合。

事务支持一次执行多个命令，一个事务中所有命令都会被序列化。

在事务执行过程，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令序列中。搜索公众号“Java精选”，回复“面试资料”。

总的来说就是redis事务就是一次性、顺序性、排他性的执行一个队列中的一系列命令。

### 题6：[什么是缓存穿透？如何避免？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题6什么是缓存穿透如何避免)<br/>
缓存穿透是指一般的缓存系统，都是按照key去缓存查询，如果不存在对应的value，就应该去后端系统查找（比如DB）。

一些恶意的请求会故意查询不存在的key，导致请求量大，造成后端系统压力大，这就是做缓存穿透。

**如何避免？**

1）对查询结果为空的情况也进行缓存，缓存时间设置短一点或key对应的数据insert后清理缓存。

2）对一定不存在的key进行过滤，可以把所有可能存在的key放到一个大的Bitmap中，查询时通过bitmap过滤。

### 题7：[Redis 中如何解决 The TCP backlog setting of 511 cannot be enforced 告警问题？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题7redis-中如何解决-the-tcp-backlog-setting-of-511-cannot-be-enforced-告警问题)<br/>
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

### 题8：[Jedis 和 Redisson 有什么优缺点？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题8jedis-和-redisson-有什么优缺点)<br/>
Jedis是Redis的Java实现的客户端，其API提供了比较全面的Redis命令的支持

Redisson实现了分布式和可扩展的Java数据结构，和Jedis相比，功能较为简单，不支持字符串操作，不支持排序、事务、管道、分区等Redis特性。

Redisson的宗旨是促进使用者对Redis的关注分离，从而让使用者能够将精力更集中地放在处理业务逻辑上。

### 题9：[Redis 持久化数据如何实现扩容？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题9redis-持久化数据如何实现扩容)<br/>
Redis当做缓存使用时，可以通过一致性哈希实现动态扩容缩容。

Redis当做持久化存储数据使用时，必须使用固定的keys-to-nodes映射关系，节点的数量一旦确定不能变化。否则的话(即Redis节点需要动态变化的情况），必须使用可以在运行时进行数据再平衡的一套系统，而当前只有Redis集群可以做到这样。

### 题10：[Redis 集群最大节点个数是多少？](/docs/Redis/最新2021年Redis面试题高级面试题及附答案解析.md#题10redis-集群最大节点个数是多少)<br/>
Redis集群最大节点个数是16384个，即哈希槽数是16384个。

Redis集群并没有使用一致性hash，而是引入了哈希槽的概念。

Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。


### 题11：redis-集群如何选择数据库<br/>


### 题12：什么是-redis<br/>


### 题13：支持一致性哈希的客户端有哪些<br/>


### 题14：为什么-redis-集群的最大槽数是-16384-个<br/>


### 题15：redis-中如何解决-overcommit_memory-is-set-to-0-告警问题<br/>


### 题16：redis-如何实现大量数据插入<br/>


### 题17：redis-回收使用的是什么算法-<br/>


### 题18：什么是-redis-持久化redis-有哪几种持久化方式<br/>


### 题19：redis-中分布式锁有什么缺陷性问题<br/>


### 题20：redis-集群会产生数据丢失情况吗<br/>


### 题21：redis-和-memcached-都有哪些区别<br/>


### 题22：redis-事务支持隔离性吗<br/>


### 题23：redis-中如何实现分布式锁<br/>


### 题24：redis-哈希槽的概念是什么<br/>


### 题25：什么是缓存雪崩何如避免<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")