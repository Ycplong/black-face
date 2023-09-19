# 最新Redis面试题及答案附答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Redis

### 题1：[为什么 Redis 需把数据放到内存中？　](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题1为什么-redis-需把数据放到内存中　)<br/>
若不将数据读到内存中，磁盘I/O速度会严重影响Redis的性能。

Redis具有快速和数据持久化的特征。

Redis为了提高读写时的速度将数据读到内存中，并通过异步的方式将数据写入磁盘。

注：若设置最大使用内存，则数据已有记录数达到内存限值后不能继续插入新值。

为什么内存读取比磁盘读取数据速度快？

1）内存是电器元件，利用高低电平存储数据，而磁盘是机械元件，电气原件速度超快，而磁盘由于在每个磁盘块切换时磁头会消耗很多时间，说白了就是IO时间长，两者性能没发比较。

2）磁盘的数据进行操作时也是读取到内存中交由CPU进行处理操作，因此直接放在内存中的数据，读取速度肯定快很多。


### 题2：[Redis 和其他key-value存储有什么不同？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题2redis-和其他key-value存储有什么不同)<br/>
Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。

Redis数据类型都是基于基本数据结构，同时对开发人员透明，无需进行额外的抽象。

Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，应为数据量不能大于硬件内存。

内存数据库方面的优点是在内存比在磁盘上相同复杂的数据结构上操作更为简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面它们是紧凑并以追加的方式产生，因为它们并不需要进行随机访问。

### 题3：[Redis 都有哪些使用场景？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题3redis-都有哪些使用场景)<br/>
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

### 题4：[Redis 事务命令都有哪几个？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题4redis-事务命令都有哪几个)<br/>
MULTI、EXEC、DISCARD、WATCH

multi：标记一个事务块的开始，返回ok。

exec：执行所有事务块内，事务块内所有命令执行的先后顺序的返回值，操作被，返回空值nil。

discard：取消事务，放弃执行事务块内的所有命令，返回ok。

watch：监视key在事务执行之前是否被其他指令改动，若已修改则事务内的指令取消执行，返回ok。

unwatch：取消watch命令对key的监视，返回ok。

### 题5：[为什么 Redis 集群的最大槽数是 16384 个？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题5为什么-redis-集群的最大槽数是-16384-个)<br/>
在redis节点发送心跳包时需要把所有的槽放到这个心跳包中，以便让节点知道当前集群信息，即16384=16k，在发送心跳包时使用char进行bitmap压缩后是2k（2*8 (8bit)*1024(1k)=16K），也就是说使用2k的空间创建了16k的槽数。

虽然使用CRC16算法最多可以分配65535（2^16-1）个槽位，即65535=65k，压缩后就是8k（8*8 (8 bit)*1024(1k)=65K），也就是说需要8k的心跳包，并且一般情况下一个redis集群不会有超过1000个master节点，所以16k的槽位是个比较合适的选择。

### 题6：[Redis 过期键都有哪些删除策略？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题6redis-过期键都有哪些删除策略)<br/>
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

### 题7：[Redis 集群如何选择数据库？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题7redis-集群如何选择数据库)<br/>
Redis集群目前无法做数据库选择，默认在0数据库。

### 题8：[什么是 Redis 事务？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题8什么是-redis-事务)<br/>
Redis事务的本质是通过MULTI、EXEC、WATCH等一组命令的集合。

事务支持一次执行多个命令，一个事务中所有命令都会被序列化。

在事务执行过程，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令序列中。搜索公众号“Java精选”，回复“面试资料”。

总的来说就是redis事务就是一次性、顺序性、排他性的执行一个队列中的一系列命令。

### 题9：[Redis 各数据类型最大容量是多少？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题9redis-各数据类型最大容量是多少)<br/>
String类型：一个String类型的value最大可以存储512M。

List类型：元素个数最多为2^32-1个，即4294967295个。

Set类型：元素个数最多为2^32-1个，即4294967295个。

Hash类型：键值对个数最多为2^32-1个，即4294967295个。

Sorted set类型：同Set类型，元素个数最多为2^32-1个，即4294967295个。

### 题10：[Redis 中如何实现分布式锁？](/docs/Redis/最新Redis面试题及答案附答案汇总.md#题10redis-中如何实现分布式锁)<br/>
分布式锁常见的三种实现方式：

1）数据库乐观锁；

2）基于Redis的分布式锁；

3）基于ZooKeeper的分布式锁。

Redis要实现分布式锁，以下条件应该得到满足：

1）互斥性

在任意时刻，只有一个客户端能持有锁。

2）不能死锁

客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。

3）容错性

只要大部分的Redis节点正常运行，客户端就可以加锁和解锁。

**一、实现**

可以通过set key value px milliseconds nx命令实现加锁， 通过Lua脚本实现解锁。
```shell
//获取锁（unique_value可以是UUID等）
SET resource_name unique_value NX PX  30000
 
//释放锁（lua脚本中，一定要比较value，防止误解锁）
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

**代码解释**

set命令要用set key value px milliseconds nx，替代setnx + expire需要分两次执行命令的方式，保证了原子性；

value要具有唯一性，可以使用UUID.randomUUID().toString()方法生成，用来标识这把锁是属于哪个请求加的，在解锁的时候就可以有依据；

释放锁时要验证 value 值，防止误解锁；

通过 Lua 脚本来避免 Check And Set 模型的并发问题，因为在释放锁的时候因为涉及到多个Redis操作 （利用了eval命令执行Lua脚本的原子性）；

**加锁代码分析**

首先，set()加入了NX参数，可以保证如果已有key存在，则函数不会调用成功，也就是只有一个客户端能持有锁，满足互斥性。其次，由于我们对锁设置了过期时间，即使锁的持有者后续发生崩溃而没有解锁，锁也会因为到了过期时间而自动解锁（即key被删除），不会发生死锁。最后，因为我们将value赋值为requestId，用来标识这把锁是属于哪个请求加的，那么在客户端在解锁的时候就可以进行校验是否是同一个客户端。

**解锁代码分析**

将Lua代码传到jedis.eval()方法里，并使参数KEYS[1]赋值为lockKey，ARGV[1]赋值为requestId。在执行的时候，首先会获取锁对应的value值，检查是否与requestId相等，如果相等则解锁（删除key）。

**存在的风险**

如果存储锁对应key的那个节点挂了的话，就可能存在丢失锁的风险，导致出现多个客户端持有锁的情况，这样就不能实现资源的独享了。

1）客户端A从master获取到锁

2）在master将锁同步到slave之前，master宕掉了（Redis的主从同步通常是异步的）。主从切换，slave节点被晋级为master节点

3）客户端B取得了同一个资源被客户端A已经获取到的另外一个锁。导致存在同一时刻存不止一个线程获取到锁的情况。

**二、redlock算法出现**

这个场景是假设有一个 redis cluster，有 5 个 redis master 实例。然后执行如下步骤获取一把锁：

1）获取当前时间戳，单位是毫秒；

2）跟上面类似，轮流尝试在每个 master 节点上创建锁，过期时间较短，一般就几十毫秒；

3）尝试在大多数节点上建立一个锁，比如 5 个节点就要求是 3 个节点 n / 2 + 1；

4）客户端计算建立好锁的时间，如果建立锁的时间小于超时时间，就算建立成功了；

5）要是锁建立失败了，那么就依次之前建立过的锁删除；

6）只要别人建立了一把分布式锁，你就得不断轮询去尝试获取锁。

Redis 官方给出了以上两种基于 Redis 实现分布式锁的方法，详细说明可以查看：

>https://redis.io/topics/distlock

**三、Redisson实现**

Redisson是一个在Redis的基础上实现的Java驻内存数据网格（In-Memory Data Grid）。它不仅提供了一系列的分布式的Java常用对象，还实现了可重入锁（Reentrant Lock）、公平锁（Fair Lock、联锁（MultiLock）、 红锁（RedLock）、 读写锁（ReadWriteLock）等，还提供了许多分布式服务。

Redisson提供了使用Redis的最简单和最便捷的方法。Redisson的宗旨是促进使用者对Redis的关注分离（Separation of Concern），从而让使用者能够将精力更集中地放在处理业务逻辑上。

**Redisson 分布式重入锁用法**

Redisson 支持单点模式、主从模式、哨兵模式、集群模式，这里以单点模式为例：
```java
// 1.构造redisson实现分布式锁必要的Config
Config config = new Config();
config.useSingleServer().setAddress("redis://127.0.0.1:5379").setPassword("123456").setDatabase(0);
// 2.构造RedissonClient
RedissonClient redissonClient = Redisson.create(config);
// 3.获取锁对象实例（无法保证是按线程的顺序获取到）
RLock rLock = redissonClient.getLock(lockKey);
try {
    /**
     * 4.尝试获取锁
     * waitTimeout 尝试获取锁的最大等待时间，超过这个值，则认为获取锁失败
     * leaseTime   锁的持有时间,超过这个时间锁会自动失效（值应设置为大于业务处理的时间，确保在锁有效期内业务能处理完）
     */
    boolean res = rLock.tryLock((long)waitTimeout, (long)leaseTime, TimeUnit.SECONDS);
    if (res) {
        //成功获得锁，在这里处理业务
    }
} catch (Exception e) {
    throw new RuntimeException("aquire lock fail");
}finally{
    //无论如何, 最后都要解锁
    rLock.unlock();
}
```

加锁流程图

![image.png](https://jingxuan.yoodb.com/upload/images/f2aeaaff795349e1a344cf0d909b60d4.png)

解锁流程图

![image.png](https://jingxuan.yoodb.com/upload/images/dfbd40a667a842849e2da6eb1a8de308.png)

可以看到RedissonLock是可重入的，并且考虑了失败重试，可以设置锁的最大等待时间，在实现上也做了一些优化，减少了无效的锁申请，提升了资源的利用率。

需要特别注意的是，RedissonLock同样没有解决 节点挂掉的时候，存在丢失锁的风险的问题。而现实情况是有一些场景无法容忍的，所以Redisson提供了实现了redlock算法的 RedissonRedLock，RedissonRedLock 真正解决了单点失败的问题，代价是需要额外的为RedissonRedLock搭建Redis环境。

因此，如果业务场景可以容忍这种小概率的错误，则推荐使用RedissonLock，如果无法容忍，则推荐使用RedissonRedLock。

参考资料：

>https://github.com/javazhiyin/advanced-java/
https://crazyfzw.github.io/2019/04/15/distributed-locks-with-redis/

### 题11：redis-如何实现大量数据插入<br/>


### 题12：redis-回收进程是如何工作的<br/>


### 题13：redis-key-如何设置过期时间和永久有效<br/>


### 题14：什么是缓存穿透如何避免<br/>


### 题15：redis-是单线程的吗为什么这么快<br/>


### 题16：redis-事务能否保证原子性是否支持回滚吗<br/>


### 题17：redis-回收使用的是什么算法-<br/>


### 题18：redis-事务支持隔离性吗<br/>


### 题19：redis-和-memcached-都有哪些区别<br/>


### 题20：什么是-redis<br/>


### 题21：为什么要用-redis-而不用-mapguava-做缓存<br/>


### 题22：redis-中分布式锁有什么缺陷性问题<br/>


### 题23：redis-哈希槽的概念是什么<br/>


### 题24：redis-如何查看使用情况及状态信息<br/>


### 题25：redis-中如何解决-the-tcp-backlog-setting-of-511-cannot-be-enforced-告警问题<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")