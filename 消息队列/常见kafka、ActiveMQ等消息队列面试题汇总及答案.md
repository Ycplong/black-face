# 常见kafka、ActiveMQ等消息队列面试题汇总及答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## 消息队列

### 题1：[如何解决 Kafka 数据丢失问题？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题1如何解决-kafka-数据丢失问题)<br/>
从Kafka的生产者与消费者的角度来看待数据丢失的问题。

**Producer**

1、retries=Long.MAX_VALUE

设置retries为一个较大的值。这里的retries同样是Producer的参数，对应前面提到的Producer自动重试。当出现网络的瞬时抖动时，消息发送可能会失败，此时配置了retries>0的Producer能够自动重试消息发送，避免消息丢失。

2、acks=all

设置acks=all。acks是Producer的一个参数，代表了你对“已提交”消息的定义。如果设置成all，则表明所有副本Broker都要接收到消息，该消息才算是“已提交”。这是最高等级的“已提交”定义。

3、max.in.flight.requests.per.connections=1

该参数指定了生产者在收到服务器晌应之前可以发送多少个消息。它的值越高，就会占用越多的内存，不过也会提升吞吐量。把它设为1可以保证消息是按照发送的顺序写入服务器的，即使发生了重试。

4、Producer要使用带有回调通知的API，也就是说不要使用producer.send(msg)，而要使用producer.send(msg,callback)。

5、其他错误处理

使用生产者内置的重试机制，可以在不造成消息丢失的情况下轻松地处理大部分错误，不过仍然需要处理其他类型的错误，例如消息大小错误、序列化错误等等。

**Consumer**

1、禁用自动提交：enable.auto.commit=false

2、消费者处理完消息之后再提交offset

3、配置auto.offset.reset

这个参数指定了在没有偏移量可提交时(比如消费者第l次启动时)或者请求的偏移量在broker上不存在时(比如数据被删了)，消费者会做些什么。

这个参数有两种配置。一种是earliest：消费者会从分区的开始位置读取数据，不管偏移量是否有效，这样会导致消费者读取大量的重复数据，但可以保证最少的数据丢失。一种是latest(默认)，如果选择了这种配置，消费者会从分区的末尾开始读取数据，这样可以减少重复处理消息，但很有可能会错过一些消息。

### 题2：[RabbitMQ 有哪些重要组件？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题2rabbitmq-有哪些重要组件)<br/>
ConnectionFactory(连接管理器)：应用程序与RabbitMQ之间建立连接的管理器

Channel(信道)：消息推送使用的通道

Exchange(交换器)：用于接受、分配消息

Queue(队列)：用于存储生产者的消息

RoutingKey(路由键)：用于把生产者的数据分配到交换器上

BindKey(绑定键)：用于把交换器的消息绑定到队列上

### 题3：[Kafka 有几种数据保留的策略？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题3kafka-有几种数据保留的策略)<br/>
Kafka有两种数据保存策略：按照过期时间保留和按照存储的消息大小保留。

### 题4：[AMQP是什么？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题4amqp是什么)<br/>
AMQP（Advanced Message Queueing Protocol）协议是一个开放的标准的的协议，它定义了系统之间如何传递消息。

AMQP不仅定义了consumer、producer、broker之间如何交互，也定义了消息的格式和命令的交换。

RabbitMQ就是AMQP协议的Erlang的实现（当然RabbitMQ还支持STOMP2、MQTT3等协议）AMQP的模型架构和RabbitMQ的模型架构是一样的，生产者将消息发送给交换器，交换器和队列绑定。

RabbitMQ中的交换器、交换器类型、队列、绑定、路由键等都是遵循的AMQP协议中相应的概念。

目前RabbitMQ最新版本默认支持的是AMQP0-9-1。

### 题5：[Kafka 中生产者和消费者的命令行是什么？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题5kafka-中生产者和消费者的命令行是什么)<br/>
生产者在主题上发布消息：

```shell
bin/kafka-console-producer.sh --broker-list 192.168.43.49:9092 --topic Hello-Kafka
```
注意这里的IP是server.properties中的 listeners的配置。接下来每个新行就是 输入一条新消息。

消费者接受消息：
```shell
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic Hello-Kafka --from-beginning
```

### 题6：[Kafka 中如何提高远程用户的吞吐量？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题6kafka-中如何提高远程用户的吞吐量)<br/>
如果用户位于与broker不同的数据中心，则可能需要调优套接口缓冲区大小，以对长网络延迟进行摊销。


### 题7：[RabbitMQ 接收到消息后必须消费吗？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题7rabbitmq-接收到消息后必须消费吗)<br/>
RabbitMQ接收到消息后可以不消费，在消息确认消费之前，可以做以下两件事：

拒绝消息消费，使用channel.basicReject(消息编号, true) 方法，消息会被分配给其他订阅者；

设置为死信队列，死信队列是用于专门存放被拒绝的消息队列。

### 题8：[Zookeeper 对于 Kafka 的作用是什么？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题8zookeeper-对于-kafka-的作用是什么)<br/>
Zookeeper是一个开放源码的、高性能的协调服务，它用于Kafka的分布式应用。

Zookeeper主要用于在集群中不同节点之间进行通信。

在Kafka中，它被用于提交偏移量，因此如果节点在任何情况下都失败了，它都可以从之前提交的偏移量中获取。

除此之外，它还执行其他活动，如:leader检测、分布式同步、配置管理、识别新节点何时离开或连接、集群、节点实时状态等等。

### 题9：[数据传输的事务定义有哪三种？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题9数据传输的事务定义有哪三种)<br/>
和 MQTT 的事务定义一样都是3种。

1）最多一次: 消息不会被重复发送，最多被传输一次，但也有可能一次不传输。

2）最少一次: 消息不会被漏发送，最少被传输一次，但也有可能被重复传输。

3）精确的一次（Exactly once）: 不会漏传输也不会重复传输，每个消息都传输被一次而且仅仅被传输一次，这是大家所期望的。

### 题10：[Kafka 中如何查看消费者组是否存在滞后消费？](/docs/消息队列/常见kafka、ActiveMQ等消息队列面试题汇总及答案.md#题10kafka-中如何查看消费者组是否存在滞后消费)<br/>
使用kafka-consumer-groups.sh命令进行查看。

```shell
$ bin/kafka-consumer-groups.sh --bootstrap-server cdh02:9092 --describe --group my-group
## 会显示下面的一些指标信息
TOPIC PARTITION CURRENT-OFFSET LOG-END-OFFSET   LAG          CONSUMER-ID HOST CLIENT-ID
主题   分区       当前offset      LEO           滞后消息数       消费者id     主机   客户端id
```

一般情况下，如果运行良好，CURRENT-OFFSET的值会和LOG-END-OFFSET的值非常接近。通过这个命令可以查看哪个分区的消费出现了滞后消费的情况。

### 题11：rabbitmq-如何保证消息顺序性<br/>


### 题12：kafka-在什么情况下回导致运行变慢<br/>


### 题13：kafka-判断一个节点是否还活着有那两个条件<br/>


### 题14：rabbitmq-有几种广播类型<br/>


### 题15：为什么使用消息队列<br/>


### 题16：kafka-中位移-offset-有什么作用<br/>


### 题17：kafka-消费者故障出现活锁问题如何解决<br/>


### 题18：kafka-消费者如何不自动提交偏移量由应用提交<br/>


### 题19：rabbitmq是什么<br/>


### 题20：如何避免消息重复投递或重复消费<br/>


### 题21：什么是消息队列<br/>


### 题22：rabbitmq-如何确保每个消息能被消费<br/>


### 题23：kafka-中使用零拷贝zero-copy有哪些场景<br/>


### 题24：kafka分布式的情况下如何保证消息的顺序消费<br/>


### 题25：rabbitmq-和-kafka-有什么区别<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")