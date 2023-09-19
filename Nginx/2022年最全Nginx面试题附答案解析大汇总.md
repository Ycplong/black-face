# 2022年最全Nginx面试题附答案解析大汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 为什么不使用多线程？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题1nginx-为什么不使用多线程)<br/>
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

### 题2：[Nginx 中是如何实现高并发？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题2nginx-中是如何实现高并发)<br/>
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

### 题3：[为什么 Nginx 要做动、静分离？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题3为什么-nginx-要做动静分离)<br/>
在我们的软件开发中，有些请求是需要后台处理的（如：.jsp,.do等等），有些请求是不需要经过后台处理的（如：css、html、jpg、js等等），这些不需要经过后台处理的文件称为静态文件，否则动态文件。

因此后台处理忽略静态文件，但是如果直接忽略静态文件的话，后台的请求次数就明显增多了。在我们对资源的响应速度有要求的时候，应该使用这种动静分离的策略去解决动、静分离将网站静态资源（HTML，JavaScript，CSS等）与后台应用分开部署，提高用户访问静态代码的速度，降低对后台应用访问。这里将静态资源放到nginx中，动态资源转发到tomcat服务器中,毕竟Tomcat的优势是处理动态请求。

### 题4：[Nginx 中如何限制并发连接数？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题4nginx-中如何限制并发连接数)<br/>
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

### 题5：[Nginx 如何处理服务请求？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题5nginx-如何处理服务请求)<br/>
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

### 题6：[Nginx 中 stub_status 和 sub_filter 指令有什么作用？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题6nginx-中-stub_status-和-sub_filter-指令有什么作用)<br/>
stub_status指令：该指令用于了解Nginx当前状态的当前状态，如当前的活动连接，接受和处理当前读/写/等待连接的总数。

sub_filter指令：它用于搜索和替换响应中的内容，并快速修复陈旧的数据。


### 题7：[Nginx 服务器解释 -s 参数有什么作用？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题7nginx-服务器解释--s-参数有什么作用)<br/>
用于运行Nginx -s参数的可执行文件。

### 题8：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题8nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题9：[Nginx 中 location指令的作用是什么？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题9nginx-中-location指令的作用是什么)<br/>
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

### 题10：[Nginx 中产生 502 错误可能原因？](/docs/Nginx/2022年最全Nginx面试题附答案解析大汇总.md#题10nginx-中产生-502-错误可能原因)<br/>
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

### 题11：为什么要使用-nginx<br/>


### 题12：nginx-中如何获得当前的时间<br/>


### 题13：nginx-中常见状态码有哪些<br/>


### 题14：nginx-和-apache-有什么区别<br/>


### 题15：nginx-中如何在-url-中保留双斜线<br/>


### 题16：nginx-中有可能将错误替换为-502503-错误吗<br/>


### 题17：nginx-中什么是-c10k-问题<br/>


### 题18：nginx-中-master-和-worker-进程分别是什么<br/>


### 题19：什么是-nginx<br/>


### 题20：nginx-如何处理http请求<br/>


### 题21：nginx-中如何配置实现高可用性<br/>


### 题22：nginx-中有多个-server{}-时先匹配哪个<br/>


### 题23：nginx-中如何禁止某ip不可访问<br/>


### 题24：nginx目录结构都有哪些<br/>


### 题25：nginx-中实现负载均衡的策略都有哪些<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")