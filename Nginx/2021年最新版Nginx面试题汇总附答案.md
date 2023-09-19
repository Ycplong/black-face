# 2021年最新版Nginx面试题汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[什么是 Nginx？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题1什么是-nginx)<br/>
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。


### 题2：[Nginx 中实现负载均衡的策略都有哪些？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题2nginx-中实现负载均衡的策略都有哪些)<br/>
为了避免服务器崩溃，一般会通过负载均衡的方式来分担服务器压力。将对台服务器组成一个集群，当用户访问时，先访问到一个转发服务器，再由转发服务器将访问分发到压力更小的服务器。

Nginx实现负载均衡的策略有五种。

**1、轮询（默认方式）**

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某个服务器宕机，则能自动剔除故障系统。

```shell
upstream jxserver { 
    server 192.168.0.1; 
    server 192.168.0.2; 
    server 192.168.0.3; 
} 
```

**2、weight（权重）**

weight的值越大分配到的访问概率越高，主要用于后端每台服务器性能不均衡的情况下或在主从的情况下设置不同的权值，达到合理有效的地利用主机资源。

```shell
upstream jxserver { 
    server 192.168.0.1 weight=2; 
    server 192.168.0.2 weight=3; 
    server 192.168.0.3 weight=5; 
}
```

**3、ip_hash（IP绑定）**

每个请求按访问IP的哈希结果分配，使得来自同一个IP的访客固定访问一台后端服务器且可以有效解决动态网页存在的session共享问题。

```shell
upstream jxserver { 
    ip_hash; 
    server 192.168.0.1:81; 
    server 192.168.0.2:82;
    server 192.168.0.3:83; 
}
```

**4、fair（第三方插件）**

Nginx必须安装upstream_fair模块。

fair算法可以根据页面大小和加载时间长短智能地进行负载均衡，响应时间（rt）短的优先分配请求。

```shell
upstream jxserver { 
    server 192.168.0.1; 
    server 192.168.0.2; 
    server 192.168.0.3; 
    fair; 
}
```

哪个服务器的响应速度快，就将请求分配到那个服务器上。

**5、url_hash（第三方插件）**

Nginx必须安装hash软件包。

与ip_hash类似，但是按照访问url的hash结果来分配请求，使得每个url定向到同一个后端服务器，主要应用于后端服务器为缓存时的场景。

```shell
upstream jxserver { 
    server 192.168.0.1; 
    server 192.168.0.2;  
    hash $request_uri; 
    hash_method crc32; 
}
```

### 题3：[Nginx 中如何限制并发连接数？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题3nginx-中如何限制并发连接数)<br/>
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

### 题4：[Nginx 中有可能将错误替换为 502、503 错误吗？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题4nginx-中有可能将错误替换为-502503-错误吗)<br/>
502 =错误网关

503 =服务器超载

Nginx中是有可能将错误替换为502、503错误，前提是必须确保fastcgi_intercept_errors被设置为ON，并使用错误页面指令。

```shell
location / {
	fastcgi_pass 127.0.01:8081;
	fastcgi_intercept_errors on;
	error_page 502 =503/error_page.html;
	#…
}
```

### 题5：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题5nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题6：[Nginx 中如何配置实现高可用性？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题6nginx-中如何配置实现高可用性)<br/>
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

### 题7：[Nginx 中如何在 URL 中保留双斜线？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题7nginx-中如何在-url-中保留双斜线)<br/>
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

### 题8：[正向代理和反向代理都有哪些区别？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题8正向代理和反向代理都有哪些区别)<br/>
**正向代理（forward proxy）**

正向代理是指一个位于客户端和目标服务器之间的服务器（代理服务器），为了从目标服务器获得内容，客户端向代理服务器发送一个请求并指定目标，然后代理服务器向目标服务器转交请求并将获得的内容返回给客户端。

正向代理通俗的来说就是一个用户发送一个请求直接就访问到目标的服务器。类似一个跳板机，代理访问外部资源。

**反向代理（Reverse Proxy）**

反向代理是指以代理服务器来接收互联网（浏览器等）上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上获得结果返回给互联网上请求连接的客户端，此时的代理服务器对外就表现为一个服务器。

反方代理也就是请求都被Nginx接收并按照一定的规则分发给了后端的业务处理服务器进行处理。

### 题9：[为什么 Nginx 性能这么高？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题9为什么-nginx-性能这么高)<br/>
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

### 题10：[Nginx 中有多个 server{} 时先匹配哪个？](/docs/Nginx/2021年最新版Nginx面试题汇总附答案.md#题10nginx-中有多个-server{}-时先匹配哪个)<br/>
如果请求同时命中多个server{}，则是从上到下匹配。

如果是分布在多个配置文件中，则通过目录中的摆放顺序，在前面的文件优先被读取。


### 题11：nginx目录结构都有哪些<br/>


### 题12：nginx-中常见状态码有哪些<br/>


### 题13：nginx-中什么是-c10k-问题<br/>


### 题14：nginx-和-apache-有什么区别<br/>


### 题15：为什么-nginx-要做动静分离<br/>


### 题16：nginx-都有哪些应用场景<br/>


### 题17：ngx_http_upstream_module-模块有什么作用<br/>


### 题18：nginx-中是如何实现高并发<br/>


### 题19：nginx-为什么不使用多线程<br/>


### 题20：nginx-中产生-502-错误可能原因<br/>


### 题21：nginx-中-stub_status-和-sub_filter-指令有什么作用<br/>


### 题22：nginx-如何处理服务请求<br/>


### 题23：nginx-如何处理http请求<br/>


### 题24：nginx-中如何禁止某ip不可访问<br/>


### 题25：nginx-中-master-和-worker-进程分别是什么<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")