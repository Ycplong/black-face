# 2021年最新版 Nginx 面试题汇总附答案

### 全部面试题答案，更新日期：12月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 中是如何实现高并发？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题1nginx-中是如何实现高并发)<br/>
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

### 题2：[为什么 Nginx 要做动、静分离？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题2为什么-nginx-要做动静分离)<br/>
在我们的软件开发中，有些请求是需要后台处理的（如：.jsp,.do等等），有些请求是不需要经过后台处理的（如：css、html、jpg、js等等），这些不需要经过后台处理的文件称为静态文件，否则动态文件。

因此后台处理忽略静态文件，但是如果直接忽略静态文件的话，后台的请求次数就明显增多了。在我们对资源的响应速度有要求的时候，应该使用这种动静分离的策略去解决动、静分离将网站静态资源（HTML，JavaScript，CSS等）与后台应用分开部署，提高用户访问静态代码的速度，降低对后台应用访问。这里将静态资源放到nginx中，动态资源转发到tomcat服务器中,毕竟Tomcat的优势是处理动态请求。

### 题3：[ngx_http_upstream_module 模块有什么作用？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题3ngx_http_upstream_module-模块有什么作用)<br/>
ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

### 题4：[Nginx 如何处理HTTP请求？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题4nginx-如何处理http请求)<br/>
Nginx使用反应器模式。

主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。

单个线程可以提供数万个并发连接。

### 题5：[Nginx 中有可能将错误替换为 502、503 错误吗？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题5nginx-中有可能将错误替换为-502503-错误吗)<br/>
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

### 题6：[Nginx 中如何获得当前的时间？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题6nginx-中如何获得当前的时间)<br/>
Nginx中要获得当前时间，必须使用SSI模块、\$date_gmt和\$date_local的变量。

```shell
Proxy_set_header THE-TIME $date_gmt;
```

### 题7：[Nginx 中什么是 C10K 问题？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题7nginx-中什么是-c10k-问题)<br/>
在传统的同步阻塞处理模型中，当创建的进程或线程过多时，缓存I/O、内核将数据拷贝到用户进程空间、阻塞，进程/线程上下文切换消耗大。

简而言之，C10K问题就是无法同时处理大量客户端（10,000）的网络套接字。

### 题8：[Nginx 中 location 匹配优先级顺序？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题8nginx-中-location-匹配优先级顺序)<br/>
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

### 题9：[Nginx 中常见状态码有哪些？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题9nginx-中常见状态码有哪些)<br/>
**状态码413**
 
request entity too large：默认nginx会限制用户长传文件大小，如果想修改限制的文件大小可以使用client_max_body_size修改。

**状态码502**

bad gateway：一般为后端服务无响应。比如反向代理到127.0.0.1，如果127.0.0.1没有响应则nginx会返给客户端502。

**状态码504**

gateway time-out：一般为后端服务执行超时，nginx默认等待时间为60秒。


### 题10：[什么是 Nginx？](/docs/Nginx/2021年最新版%20Nginx%20面试题汇总附答案.md#题10什么是-nginx)<br/>
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")