# 66道Nginx经典面试题（面试率高）

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[为什么 Nginx 性能这么高？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题1为什么-nginx-性能这么高)<br/>
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

### 题2：[ngx_http_upstream_module 模块有什么作用？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题2ngx_http_upstream_module-模块有什么作用)<br/>
ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

### 题3：[Nginx 中常见状态码有哪些？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题3nginx-中常见状态码有哪些)<br/>
**状态码413**
 
request entity too large：默认nginx会限制用户长传文件大小，如果想修改限制的文件大小可以使用client_max_body_size修改。

**状态码502**

bad gateway：一般为后端服务无响应。比如反向代理到127.0.0.1，如果127.0.0.1没有响应则nginx会返给客户端502。

**状态码504**

gateway time-out：一般为后端服务执行超时，nginx默认等待时间为60秒。


### 题4：[Nginx 中如何限制并发连接数？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题4nginx-中如何限制并发连接数)<br/>
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

### 题5：[Nginx 中 location 匹配优先级顺序？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题5nginx-中-location-匹配优先级顺序)<br/>
在开始处理一个http请求时，nginx会取出header头中的host，与nginx.conf中每个server的server_name进行匹配，以此决定到底由哪一个server块来处理这个请求。

location匹配优先级为= > ^ > ~* = ~。

=：表示进行普通字符精确匹配，也就是完全匹配。

^~：表示普通字符匹配，使用前缀匹配。

~*：表示执行一个正则匹配（不区分大小写）。

~：表示执行一个正则匹配（区分大小写）。

server_name与host匹配优先级如下：

1、完全匹配

2、通配符在前的，如*.test.com

3、在后的，如www.test.*

4、正则匹配，如~^\.www\.test\.com$

如果都无法匹配时

1、优先选择listen配置项后有default或default_server。

2、找到匹配listen端口的第一个server块。

### 题6：[为什么要使用 Nginx？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题6为什么要使用-nginx)<br/>
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


### 题7：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题7nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题8：[什么是 Nginx？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题8什么是-nginx)<br/>
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。


### 题9：[Nginx 中如何在 URL 中保留双斜线？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题9nginx-中如何在-url-中保留双斜线)<br/>
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

### 题10：[Nginx 如何处理服务请求？](/docs/Nginx/66道Nginx经典面试题（面试率高）.md#题10nginx-如何处理服务请求)<br/>
Nginx接收某个请求后，首先由listen和server_name指令匹配server模块，然后再匹配server模块中的location，location也就是实际访问请求。

```shell
server {            					
	listen       80；      				
	server_name  localhost;
	location  / {
		root   html;
		index  index.html index.htm;
}
```

http模块支持嵌套多个server，配置代理、缓存、日志定义等大多数功能和第三方模块的配置。例如文件引入、mime-type定义、日志自定义、是否使用sendfile传输文件、连接超时时间、单连接请求数等。

server模块配置虚拟主机的相关参数，表示一个独立的虚拟主机站点。

listen参数提供服务的端口，默认80端口。

server_name参数提供服务的域名主机名称。

location模块配置请求的路由，以及各种页面的处理情况。

root参数站点的根目录，相当于Nginx的安装目录。

index参数默认首页文件，多个文件可用空格分开。

### 题11：nginx-中有多个-server{}-时先匹配哪个<br/>


### 题12：nginx-如何处理http请求<br/>


### 题13：nginx-中如何禁止某ip不可访问<br/>


### 题14：nginx-中-stub_status-和-sub_filter-指令有什么作用<br/>


### 题15：nginx-中什么是-c10k-问题<br/>


### 题16：nginx-中有可能将错误替换为-502503-错误吗<br/>


### 题17：nginx-中如何配置实现高可用性<br/>


### 题18：为什么-nginx-要做动静分离<br/>


### 题19：nginx-中产生-502-错误可能原因<br/>


### 题20：nginx-中实现负载均衡的策略都有哪些<br/>


### 题21：nginx-中-master-和-worker-进程分别是什么<br/>


### 题22：nginx-都有哪些应用场景<br/>


### 题23：nginx-中如何获得当前的时间<br/>


### 题24：nginx-中是如何实现高并发<br/>


### 题25：nginx-为什么不使用多线程<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")