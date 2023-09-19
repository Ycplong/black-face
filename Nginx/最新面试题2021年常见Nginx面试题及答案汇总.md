# 最新面试题2021年常见Nginx面试题及答案汇总

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 中常见状态码有哪些？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题1nginx-中常见状态码有哪些)<br/>
**状态码413**
 
request entity too large：默认nginx会限制用户长传文件大小，如果想修改限制的文件大小可以使用client_max_body_size修改。

**状态码502**

bad gateway：一般为后端服务无响应。比如反向代理到127.0.0.1，如果127.0.0.1没有响应则nginx会返给客户端502。

**状态码504**

gateway time-out：一般为后端服务执行超时，nginx默认等待时间为60秒。


### 题2：[Nginx 如何处理HTTP请求？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题2nginx-如何处理http请求)<br/>
Nginx使用反应器模式。

主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。

单个线程可以提供数万个并发连接。

### 题3：[Nginx 中 location 匹配优先级顺序？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题3nginx-中-location-匹配优先级顺序)<br/>
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

### 题4：[Nginx 中有多个 server{} 时先匹配哪个？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题4nginx-中有多个-server{}-时先匹配哪个)<br/>
如果请求同时命中多个server{}，则是从上到下匹配。

如果是分布在多个配置文件中，则通过目录中的摆放顺序，在前面的文件优先被读取。


### 题5：[Nginx 中如何禁止某IP不可访问？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题5nginx-中如何禁止某ip不可访问)<br/>
Nginx配置禁止192.168.0.1不可访问，修改nginx.conf文件，在server中添加deny ip配置如下：

```shell
server {
  listen       80;
  server_name  localhost;
 
  charset utf8;
 
  deny  192.168.0.1;
 
  location / {
    proxy_pass  http://blog.yoodb.com/;
    proxy_set_header Host      $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```

### 题6：[Nginx 中是如何实现高并发？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题6nginx-中是如何实现高并发)<br/>
一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。

但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。

那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。

这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

### 题7：[什么是 Nginx？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题7什么是-nginx)<br/>
Nginx（engine x）是一个轻量级、高性能的HTTP和反向代理web服务器，同时也是一个IMAP、POP3、SMTP代理服务器。

Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

Nginx相比较Apache、lighttpd具有占有内存少，稳定性高等优势，并且依靠并发能力强，丰富的模块库以及友好灵活的配置而闻名。


### 题8：[Nginx目录结构都有哪些？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题8nginx目录结构都有哪些)<br/>
```shell
├── client_body_temp
├── conf                             # Nginx所有配置文件目录
│   ├── fastcgi.conf                 # fastcgi相关参数配置文件
│   ├── fastcgi.conf.default         # fastcgi.conf原始备份文件
│   ├── fastcgi_params               # fastcgi参数文件
│   ├── fastcgi_params.default       
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types                   # 媒体类型
│   ├── mime.types.default
│   ├── nginx.conf                   # Nginx主配置文件
│   ├── nginx.conf.default
│   ├── scgi_params                  # scgi相关参数文件
│   ├── scgi_params.default  
│   ├── uwsgi_params                 # uwsgi相关参数文件
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp                     # fastcgi临时数据目录
├── html                             # Nginx默认站点目录
│   ├── 50x.html                     # 错误页面
│   └── index.html                   # 默认首页文件
├── logs                             # Nginx日志目录
│   ├── access.log                   # 访问日志文件
│   ├── error.log                    # 错误日志文件
│   └── nginx.pid                    # pid文件，Nginx进程启动后会把所有进程ID号写到此文件
├── proxy_temp                       # 临时目录
├── sbin                             # Nginx命令目录
│   └── nginx                        # Nginx启动命令
├── scgi_temp                        # 临时目录
└── uwsgi_temp                       # 临时目录
```

### 题9：[Nginx 中如何在 URL 中保留双斜线？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题9nginx-中如何在-url-中保留双斜线)<br/>
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

### 题10：[Nginx 中 location指令的作用是什么？](/docs/Nginx/最新面试题2021年常见Nginx面试题及答案汇总.md#题10nginx-中-location指令的作用是什么)<br/>
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

### 题11：nginx-中产生-502-错误可能原因<br/>


### 题12：nginx-为什么不使用多线程<br/>


### 题13：为什么要使用-nginx<br/>


### 题14：nginx-中如何限制并发连接数<br/>


### 题15：nginx-都有哪些应用场景<br/>


### 题16：为什么-nginx-要做动静分离<br/>


### 题17：nginx-和-apache-有什么区别<br/>


### 题18：nginx-中-master-和-worker-进程分别是什么<br/>


### 题19：nginx-中如何获得当前的时间<br/>


### 题20：nginx-中有可能将错误替换为-502503-错误吗<br/>


### 题21：nginx-中实现负载均衡的策略都有哪些<br/>


### 题22：nginx-中如何解决前端跨域问题<br/>


### 题23：nginx-如何处理服务请求<br/>


### 题24：nginx-中什么是-c10k-问题<br/>


### 题25：nginx-服务器解释--s-参数有什么作用<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")