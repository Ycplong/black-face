# 2022年最全Redis面试题附答案解析大汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[为什么要用 Redis 而不用 Map、Guava 做缓存？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题1为什么要用-redis-而不用-mapguava-做缓存)<br/>
缓存可以划分为本地缓存和分布式缓存。

以Java为例，使用自带Map或者Guava类实现的是本地缓存，主要特点是轻量以及快速，它们的生命周期会随着JVM的销毁而结束，且在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。

使用Redis或Memcached等被称为分布式缓存，在多实例的情况下，各实例共用一份缓存数据，具有一致性，但是需要保持Redis或Memcached服务的高可用。

### 题2：[什么是缓存雪崩？何如避免？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题2什么是缓存雪崩何如避免)<br/>
缓存雪崩是指当缓存服务器重启或大量缓存集中在某一个时间段内失效，这样在失效时，会给后端系统带来很大压力，导致系统崩溃。

**如何避免？**

1）在缓存失效后，使用加锁或队列的方式来控制读数据库写缓存的线程数量。如对某个key只允许一个线程查询数据和写缓存，其他线程等待。

2）实现二级缓存方式，A1为原始缓存，A2为拷贝缓存，A1失效时，切换访问A2，A1缓存失效时间设置为短期，A2设置为长期。

3）不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。

### 题3：[Redis 事务命令都有哪几个？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题3redis-事务命令都有哪几个)<br/>
MULTI、EXEC、DISCARD、WATCH

multi：标记一个事务块的开始，返回ok。

exec：执行所有事务块内，事务块内所有命令执行的先后顺序的返回值，操作被，返回空值nil。

discard：取消事务，放弃执行事务块内的所有命令，返回ok。

watch：监视key在事务执行之前是否被其他指令改动，若已修改则事务内的指令取消执行，返回ok。

unwatch：取消watch命令对key的监视，返回ok。

### 题4：[Redis 如何处理数据存储实现内存优化？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题4redis-如何处理数据存储实现内存优化)<br/>
尽量使用redis的散列表，把相关的信息放到散列表里面存储，而不是把每个字段单独存储，这样可以有效的减少内存使用。

可以通过使用Hash、list、sorted set、set等集合类型数据，因为通常情况下很多小的Key-Value可以用更紧凑的方式存放到一起。尽可能使用散列表（hashes），散列表（是说散列表里面存储的数少）使用的内存非常小，所以你应该尽可能的将你的数据模型抽象到一个散列表里面。

比如web系统中有一个用户对象，不要为这个用户的名称，姓氏，邮箱，密码设置单独的key，而是把这个用户的所有信息存储到一张散列表中。

### 题5：[Redis主要占用什么物理资源？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题5redis主要占用什么物理资源)<br/>
内存资源。

### 题6：[Redis 各数据类型最大容量是多少？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题6redis-各数据类型最大容量是多少)<br/>
String类型：一个String类型的value最大可以存储512M。

List类型：元素个数最多为2^32-1个，即4294967295个。

Set类型：元素个数最多为2^32-1个，即4294967295个。

Hash类型：键值对个数最多为2^32-1个，即4294967295个。

Sorted set类型：同Set类型，元素个数最多为2^32-1个，即4294967295个。

### 题7：[Redis 如何测试连通性？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题7redis-如何测试连通性)<br/>
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


### 题8：[为什么 Redis 需把数据放到内存中？　](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题8为什么-redis-需把数据放到内存中　)<br/>
若不将数据读到内存中，磁盘I/O速度会严重影响Redis的性能。

Redis具有快速和数据持久化的特征。

Redis为了提高读写时的速度将数据读到内存中，并通过异步的方式将数据写入磁盘。

注：若设置最大使用内存，则数据已有记录数达到内存限值后不能继续插入新值。

为什么内存读取比磁盘读取数据速度快？

1）内存是电器元件，利用高低电平存储数据，而磁盘是机械元件，电气原件速度超快，而磁盘由于在每个磁盘块切换时磁头会消耗很多时间，说白了就是IO时间长，两者性能没发比较。

2）磁盘的数据进行操作时也是读取到内存中交由CPU进行处理操作，因此直接放在内存中的数据，读取速度肯定快很多。


### 题9：[什么是 Redis 持久化？Redis 有哪几种持久化方式？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题9什么是-redis-持久化redis-有哪几种持久化方式)<br/>
Redis持久化是指把内存的数据写到磁盘中去，防止服务因宕机导致内存数据丢失。

Redis提供了两种持久化方式：RDB（默认）和AOF。

RDB是Redis DataBase缩写，功能核心函数rdbSave（生成RDB文件）和rdbLoad（从文件加载内存）两个函数。

AOF是Append-only file缩写，每当执行服务器（定时）任务或者函数时flushAppendOnlyFile函数都会被调用，这个函数执行以下两个工作。

**AOF写入与保存**

WRITE：根据条件，将aof_buf中的缓存写入到AOF文件

SAVE：根据条件，调用fsync或fdatasync函数，将AOF文件保存到磁盘中。

### 题10：[Redis 事务支持隔离性吗？](/docs/Redis/2022年最全Redis面试题附答案解析大汇总.md#题10redis-事务支持隔离性吗)<br/>
Redis是单进程程序且保证在执行事务时，不会对事务进行中断，事务可以运行直到执行完所有事务队列中的命令为止。因此，Redis的事务是总是带有隔离性的。

### 题11：redis-中如何解决-thp-服务导致的延迟和内存使用问题<br/>


### 题12：redis-和其他key-value存储有什么不同<br/>


### 题13：redis-将内存占满后会发生什么问题<br/>


### 题14：什么是-redis<br/>


### 题15：jedis-和-redisson-有什么优缺点<br/>


### 题16：redis-中如何实现分布式锁<br/>


### 题17：redis-中分布式锁有什么缺陷性问题<br/>


### 题18：redis-集群如何选择数据库<br/>


### 题19：redis-回收使用的是什么算法-<br/>


### 题20：redis-如何设置密码及验证密码<br/>


### 题21：redis-集群会产生数据丢失情况吗<br/>


### 题22：redis-官方为什么不提供-windows-版本<br/>


### 题23：支持一致性哈希的客户端有哪些<br/>


### 题24：redis-事务能否保证原子性是否支持回滚吗<br/>


### 题25：redis-过期键都有哪些删除策略<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")