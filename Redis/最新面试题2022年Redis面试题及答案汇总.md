# 最新面试题2022年Redis面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[Redis 是单线程的，如何提高多核 CPU 的利用率？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题1redis-是单线程的如何提高多核-cpu-的利用率)<br/>
可以在同一个服务器部署多个Redis的实例，并把他们当作不同的服务器来使用，在某些时候，无论如何一个服务器是不够的， 所以，如果你想使用多个CPU，你可以考虑一下分片(shard)。

### 题2：[Redis 中如何解决 overcommit_memory is set to 0 告警问题？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题2redis-中如何解决-overcommit_memory-is-set-to-0-告警问题)<br/>
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

### 题3：[Redis 如何测试连通性？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题3redis-如何测试连通性)<br/>
redis中使用ping ip命令测试连通性。

方式一：使用ping指令

```shell
redis-cli -h host -p port -a password
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

方式二：Java代码中实现Redis连通性测试，可以使用Redis客户端类库包里的api发送ping指令

```java
//连接redis
Jedis jedis=new Jedis("127.0.0.1",6379);
//查看服务器是否运行，打出 pong 表示OK
System.out.println("ping redis：" + jedis.ping());
```


### 题4：[Redis 哈希槽的概念是什么？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题4redis-哈希槽的概念是什么)<br/>
Redis集群没有使用一致性hash，而是引入了哈希槽的概念，Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。

### 题5：[什么是 Redis？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题5什么是-redis)<br/>
redis是一个高性能的key-value数据库，它是完全开源免费的，而且redis是一个NOSQL类型数据库，是为了解决高并发、高扩展，大数据存储等一系列的问题而产生的数据库解决方案，是一个非关系型的数据库。但是，它也是不能替代关系型数据库，只能作为特定环境下的扩充。

redis是一个以key-value存储的数据库结构型服务器，它支持的数据结构类型包括：字符串（String）、链表（lists）、哈希表（hash）、集合（set）、有序集合（Zset）等。为了保证读取的效率，redis把数据对象都存储在内存当中，它可以支持周期性的把更新的数据写入磁盘文件中。而且它还提供了交集和并集，以及一些不同方式排序的操作。

### 题6：[Redis 集群最大节点个数是多少？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题6redis-集群最大节点个数是多少)<br/>
Redis集群最大节点个数是16384个，即哈希槽数是16384个。

Redis集群并没有使用一致性hash，而是引入了哈希槽的概念。

Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。


### 题7：[为什么 Redis 集群的最大槽数是 16384 个？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题7为什么-redis-集群的最大槽数是-16384-个)<br/>
在redis节点发送心跳包时需要把所有的槽放到这个心跳包中，以便让节点知道当前集群信息，即16384=16k，在发送心跳包时使用char进行bitmap压缩后是2k（2*8 (8bit)*1024(1k)=16K），也就是说使用2k的空间创建了16k的槽数。

虽然使用CRC16算法最多可以分配65535（2^16-1）个槽位，即65535=65k，压缩后就是8k（8*8 (8 bit)*1024(1k)=65K），也就是说需要8k的心跳包，并且一般情况下一个redis集群不会有超过1000个master节点，所以16k的槽位是个比较合适的选择。

### 题8：[Redis 和 Memcached 都有哪些区别？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题8redis-和-memcached-都有哪些区别)<br/>
1）Redis是单进程单线程的。使用了I/O多路复用器，高并发情况下不存在数据安全问题；而Memcached支持多线程。

2）Redis存储K-V结构的数据，Value支持多种数据类型，比如String、Hash、Set、SortedSet、List类型；而Memcached仅支持简单的K-V结构的数据。

3）Redis支持数据持久化，服务器重启后数据可以恢复；而Memcached不支持数据持久化，服务器重启后数据无法恢复。

4）Redis中List类型支持排序；而Memcached不支持排序。

5）Redis中value值最大可以存储512MB；而Memcached中key的最大长度为255个字符，value最大可以存储1MB。

6）Memcached和Redis在数据的写入上效率基本相差无几，但是在数据的读取尤其是批量数据的读取时，Memcached的效率更高。

Redis使用单核，而Memcached可以使用多核，平均每一个核上Redis在存储小数据时比Memcached性能更高。而在100k以上的数据中，Memcached性能要高于Redis。虽然Redis最近在存储大数据的性能上进行优化，但是比起Memcachedd，还是稍有逊色。

### 题9：[Redis主要占用什么物理资源？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题9redis主要占用什么物理资源)<br/>
内存资源。

### 题10：[Redis key 如何设置过期时间和永久有效？](/docs/Redis/最新面试题2022年Redis面试题及答案汇总.md#题10redis-key-如何设置过期时间和永久有效)<br/>
Redis key设置过期时间使用EXPIRE命令。

Redis key设置永久有效使用PERSIST命令。

### 题11：redis-回收进程是如何工作的<br/>


### 题12：redis-事务支持隔离性吗<br/>


### 题13：redis-和-redisson-有什么区别和关系<br/>


### 题14：redis-是单线程的吗为什么这么快<br/>


### 题15：redis-集群如何选择数据库<br/>


### 题16：redis-集群会产生数据丢失情况吗<br/>


### 题17：redis-事务能否保证原子性是否支持回滚吗<br/>


### 题18：redis-中如何解决-the-tcp-backlog-setting-of-511-cannot-be-enforced-告警问题<br/>


### 题19：redis-中如何实现分布式锁<br/>


### 题20：redis-将内存占满后会发生什么问题<br/>


### 题21：redis-回收使用的是什么算法-<br/>


### 题22：redis-集群之间是如何复制的<br/>


### 题23：什么是-redis-持久化redis-有哪几种持久化方式<br/>


### 题24：为什么要用-redis-而不用-mapguava-做缓存<br/>


### 题25：redis-中分布式锁有什么缺陷性问题<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")