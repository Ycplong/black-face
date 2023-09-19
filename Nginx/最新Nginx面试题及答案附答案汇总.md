# 最新Nginx面试题及答案附答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 中如何在 URL 中保留双斜线？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题1nginx-中如何在-url-中保留双斜线)<br/>
要在URL中保留双斜线，就必须使用merge_slashes参数。

语法:merge_slashes [on/off]

默认值: merge_slashes on

可以在http、server模块中使用。nginx配置信息组织结构如下：
>http 
     |__server
     |     |__location
     |     |__location
     |
     |__server
           |__location

### 题2：[为什么要使用 Nginx？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题2为什么要使用-nginx)<br/>
1、跨平台、配置简单。

2、非阻塞、高并发连接：处理2-3万并发连接数，官方监测能支持5万并发。

3、内存消耗小：开启10个nginx才占150M内存，Nginx采取了分阶段资源分配技术。

4、nginx处理静态文件好,耗费内存少。

5、内置的健康检查功能：如果有一个服务器宕机，会做一个健康检查，再发送的请求就不会发送到宕机的服务器了。重新将请求提交到其他的节点上。

6、节省宽带：支持GZIP压缩，可以添加浏览器本地缓存。

7、稳定性高：宕机的概率非常小。

8、master/worker结构：一个master进程，生成一个或者多个worker进程。

9、接收用户请求是异步的：浏览器将请求发送到nginx服务器，它先将用户请求全部接收下来，再一次性发送给后端web服务器，极大减轻了web服务器的压力，一边接收web服务器的返回数据，一边发送给浏览器客户端。

10、网络依赖性比较低，只要ping通就可以负载均衡。

11、可以有多台nginx服务器。

**总体来说：**

Nginx支持跨平台、高并发，配置简单，实现了非常高效的反向代理、负载平衡，它可以处理2-3万并发连接数，官方监测可支持5万并发，目前我国使用Nginx有很多知名网站。

Nginx内存占用少，处理静态文件速度快且Nginx内置了健康检查功能，也就是如果有一个服务器宕机，会做一个健康检查，再发送请求时就不会发送到宕机的服务器上，Nginx会重新将请求提交到其他的节点上。

Nginx对宽带要求低，支持GZIP压缩，还可以添加浏览器本地缓存；其稳定性高，存在宕机的概率非常小，支持异步接收用户请求。


### 题3：[Nginx 和 apache 有什么区别？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题3nginx-和-apache-有什么区别)<br/>
Nginx轻量级，同样是web服务，比apache 占用更少的内存及资源；

抗并发，nginx处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能；

高度模块化的设计，编写模块相对简单；最核心的区别在于apache是同步多进程模型，一个连接对应一个进程；

nginx是异步的，多个连接（万级别）可以对应一个进程。

### 题4：[Nginx 都有哪些应用场景？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题4nginx-都有哪些应用场景)<br/>
**http服务器**

Nginx是一个http服务可以独立提供http服务，也可以做网页静态服务器。

**虚拟主机**

可以实现在一台服务器虚拟出多个网站，例如个人网站使用的虚拟机。

**反向代理，负载均衡**

当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用Nginx作为反向代理。并且多台服务器可以平均分担负载，不会出现某台服务器负载高宕机而某台服务器闲置的情况。

**配置安全管理**

使用Nginx搭建API接口网关，对每个接口服务进行拦截。

### 题5：[Nginx 中如何限制并发连接数？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题5nginx-中如何限制并发连接数)<br/>
Nginx中ngx_http_limit_conn_module模块提供了限制并发连接数的功能，可以使用limit_conn_zone指令以及limit_conn执行进行配置。

实例配置如下：

```shell
http {
    limit_conn_zone $binary_remote_addr zone=myip:10m;
    limit_conn_zone $server_name zone=myServerName:10m;
    server {
        location / {
            limit_conn myip 10;
            limit_conn myServerName 100;
            rewrite / http://www.lijie.net permanent;
        }
    }
}
```

### 题6：[Nginx 如何处理HTTP请求？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题6nginx-如何处理http请求)<br/>
Nginx使用反应器模式。

主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。

单个线程可以提供数万个并发连接。

### 题7：[Nginx 中是如何实现高并发？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题7nginx-中是如何实现高并发)<br/>
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

### 题8：[Nginx 为什么不使用多线程？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题8nginx-为什么不使用多线程)<br/>
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

### 题9：[Nginx 中如何配置实现高可用性？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题9nginx-中如何配置实现高可用性)<br/>
当上游服务器即真实访问服务器，一旦出现故障或者是没有及时响应的情况下，应该直接轮训到下一台服务器，从而保证服务器的高可用。

nginx中nginx.conf配置文件，部分内容如下：

```shell
server {
	listen       80;
	server_name  blog.yoodb.com;
	location / {
		proxy_pass http://127.0.0.1:8081;
		proxy_connect_timeout 1s;
		proxy_send_timeout 1s;
		proxy_read_timeout 1s;
		index  index.html index.htm;
	}
}
```

其中参数含义如下：

**proxy_pass**

指定上游服务器负载均衡服务器

**proxy_connect_timeout**

与上游服务器超时时间，后端服务器连接的超时时间，发起握手等候响应超时时间

**proxy_send_timeout**

nginx发送给上游服务器的超时时间

**proxy_read_timeout**

nginx接收上游服务器的超时时间

### 题10：[Nginx 中 location指令的作用是什么？](/docs/Nginx/最新Nginx面试题及答案附答案汇总.md#题10nginx-中-location指令的作用是什么)<br/>
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

### 题11：为什么-nginx-要做动静分离<br/>


### 题12：nginx目录结构都有哪些<br/>


### 题13：nginx-中-master-和-worker-进程分别是什么<br/>


### 题14：nginx-中常见状态码有哪些<br/>


### 题15：nginx-中有可能将错误替换为-502503-错误吗<br/>


### 题16：nginx-服务器解释--s-参数有什么作用<br/>


### 题17：nginx-中产生-502-错误可能原因<br/>


### 题18：nginx-中什么是-c10k-问题<br/>


### 题19：nginx-中有多个-server{}-时先匹配哪个<br/>


### 题20：为什么-nginx-性能这么高<br/>


### 题21：nginx-中-location-匹配优先级顺序<br/>


### 题22：什么是-nginx<br/>


### 题23：nginx-中如何解决前端跨域问题<br/>


### 题24：nginx-中-stub_status-和-sub_filter-指令有什么作用<br/>


### 题25：正向代理和反向代理都有哪些区别<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")