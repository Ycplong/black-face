# 2022年初，常见Nginx面试题汇总及答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[为什么 Nginx 性能这么高？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题1为什么-nginx-性能这么高)<br/>
Nginx支持异步非阻塞事件处理机制，使用最新的epoll（Linux 2.6内核）和kqueue（freebsd）网络I/O模型。

目前Linux下能够承受高并发访问的Squid、Memcached都是采用的epoll网络I/O模型。

epoll的接口共有三个函数：

```shell
int epoll_create(int size);
```

创建一个epoll的句柄，size参数是指内核监听的数目。

```shell
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
```

epoll的事件注册函数，select()是在监听事件时告知内核要监听什么类型事件，而该函数是先注册要监听的事件类型。

```shell
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

等待事件的产生，类似于select()调用。

### 题2：[Nginx 中 location指令的作用是什么？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题2nginx-中-location指令的作用是什么)<br/>
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

### 题3：[Nginx 中 Master 和 Worker 进程分别是什么？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题3nginx-中-master-和-worker-进程分别是什么)<br/>
Master进程：读取及评估配置和维持。

Worker进程：处理请求。

### 题4：[Nginx 为什么不使用多线程？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题4nginx-为什么不使用多线程)<br/>
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

### 题5：[Nginx 中如何限制并发连接数？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题5nginx-中如何限制并发连接数)<br/>
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

### 题6：[Nginx 都有哪些应用场景？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题6nginx-都有哪些应用场景)<br/>
**http服务器**

Nginx是一个http服务可以独立提供http服务，也可以做网页静态服务器。

**虚拟主机**

可以实现在一台服务器虚拟出多个网站，例如个人网站使用的虚拟机。

**反向代理，负载均衡**

当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用Nginx作为反向代理。并且多台服务器可以平均分担负载，不会出现某台服务器负载高宕机而某台服务器闲置的情况。

**配置安全管理**

使用Nginx搭建API接口网关，对每个接口服务进行拦截。

### 题7：[为什么 Nginx 要做动、静分离？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题7为什么-nginx-要做动静分离)<br/>
在我们的软件开发中，有些请求是需要后台处理的（如：.jsp,.do等等），有些请求是不需要经过后台处理的（如：css、html、jpg、js等等），这些不需要经过后台处理的文件称为静态文件，否则动态文件。

因此后台处理忽略静态文件，但是如果直接忽略静态文件的话，后台的请求次数就明显增多了。在我们对资源的响应速度有要求的时候，应该使用这种动静分离的策略去解决动、静分离将网站静态资源（HTML，JavaScript，CSS等）与后台应用分开部署，提高用户访问静态代码的速度，降低对后台应用访问。这里将静态资源放到nginx中，动态资源转发到tomcat服务器中,毕竟Tomcat的优势是处理动态请求。

### 题8：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题8nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题9：[ngx_http_upstream_module 模块有什么作用？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题9ngx_http_upstream_module-模块有什么作用)<br/>
ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

### 题10：[Nginx 中是如何实现高并发？](/docs/Nginx/2022年初，常见Nginx面试题汇总及答案.md#题10nginx-中是如何实现高并发)<br/>
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

### 题11：nginx-中常见状态码有哪些<br/>


### 题12：nginx-和-apache-有什么区别<br/>


### 题13：nginx-中如何配置实现高可用性<br/>


### 题14：什么是-nginx<br/>


### 题15：正向代理和反向代理都有哪些区别<br/>


### 题16：nginx-中如何在-url-中保留双斜线<br/>


### 题17：nginx-如何处理http请求<br/>


### 题18：nginx-中如何禁止某ip不可访问<br/>


### 题19：nginx-中实现负载均衡的策略都有哪些<br/>


### 题20：nginx-如何处理服务请求<br/>


### 题21：nginx-中有多个-server{}-时先匹配哪个<br/>


### 题22：nginx-中什么是-c10k-问题<br/>


### 题23：nginx-中如何获得当前的时间<br/>


### 题24：nginx-中产生-502-错误可能原因<br/>


### 题25：nginx-中有可能将错误替换为-502503-错误吗<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")