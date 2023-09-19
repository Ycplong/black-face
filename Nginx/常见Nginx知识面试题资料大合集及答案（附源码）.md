# 常见Nginx知识面试题资料大合集及答案（附源码）

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 中如何配置实现高可用性？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题1nginx-中如何配置实现高可用性)<br/>
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

### 题2：[Nginx 中如何获得当前的时间？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题2nginx-中如何获得当前的时间)<br/>
Nginx中要获得当前时间，必须使用SSI模块、\$date_gmt和\$date_local的变量。

```shell
Proxy_set_header THE-TIME $date_gmt;
```

### 题3：[Nginx 中产生 502 错误可能原因？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题3nginx-中产生-502-错误可能原因)<br/>
1、FastCGI进程是否已经启动

2、FastCGI worker进程数是否不够

3、FastCGI执行时间过长

1、fastcgi_connect_timeout 300;

2、fastcgi_send_timeout 300;

3、fastcgi_read_timeout 300;

FastCGI Buffer不够

1、nginx和apache一样，有前端缓冲限制，可以调整缓冲参数

2、fastcgi_buffer_size 32k;

3、fastcgi_buffers 8 32k;

Proxy Buffer不够

1、如果你用了Proxying，调整

2、proxy_buffer_size 16k;

3、proxy_buffers 4 16k;

php脚本执行时间过长

将php-fpm.conf的0s的0s改成一个时间。

### 题4：[正向代理和反向代理都有哪些区别？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题4正向代理和反向代理都有哪些区别)<br/>
**正向代理（forward proxy）**

正向代理是指一个位于客户端和目标服务器之间的服务器（代理服务器），为了从目标服务器获得内容，客户端向代理服务器发送一个请求并指定目标，然后代理服务器向目标服务器转交请求并将获得的内容返回给客户端。

正向代理通俗的来说就是一个用户发送一个请求直接就访问到目标的服务器。类似一个跳板机，代理访问外部资源。

**反向代理（Reverse Proxy）**

反向代理是指以代理服务器来接收互联网（浏览器等）上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上获得结果返回给互联网上请求连接的客户端，此时的代理服务器对外就表现为一个服务器。

反方代理也就是请求都被Nginx接收并按照一定的规则分发给了后端的业务处理服务器进行处理。

### 题5：[Nginx 为什么不使用多线程？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题5nginx-为什么不使用多线程)<br/>
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

### 题6：[Nginx 如何处理HTTP请求？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题6nginx-如何处理http请求)<br/>
Nginx使用反应器模式。

主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。

单个线程可以提供数万个并发连接。

### 题7：[ngx_http_upstream_module 模块有什么作用？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题7ngx_http_upstream_module-模块有什么作用)<br/>
ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

### 题8：[什么是 Nginx？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题8什么是-nginx)<br/>
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。


### 题9：[Nginx 如何处理服务请求？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题9nginx-如何处理服务请求)<br/>
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

### 题10：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/常见Nginx知识面试题资料大合集及答案（附源码）.md#题10nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题11：nginx-中-master-和-worker-进程分别是什么<br/>


### 题12：nginx-中常见状态码有哪些<br/>


### 题13：nginx-中如何禁止某ip不可访问<br/>


### 题14：nginx-中什么是-c10k-问题<br/>


### 题15：nginx-都有哪些应用场景<br/>


### 题16：为什么-nginx-要做动静分离<br/>


### 题17：nginx-中如何限制并发连接数<br/>


### 题18：nginx-中-location-匹配优先级顺序<br/>


### 题19：nginx-中有多个-server{}-时先匹配哪个<br/>


### 题20：nginx-服务器解释--s-参数有什么作用<br/>


### 题21：nginx-中是如何实现高并发<br/>


### 题22：nginx-中-stub_status-和-sub_filter-指令有什么作用<br/>


### 题23：为什么要使用-nginx<br/>


### 题24：nginx-中实现负载均衡的策略都有哪些<br/>


### 题25：nginx目录结构都有哪些<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")