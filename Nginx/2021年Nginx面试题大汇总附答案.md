# 2021年Nginx面试题大汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Nginx

### 题1：[Nginx 中实现负载均衡的策略都有哪些？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题1nginx-中实现负载均衡的策略都有哪些)<br/>
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

### 题2：[Nginx 中 location 匹配优先级顺序？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题2nginx-中-location-匹配优先级顺序)<br/>
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

### 题3：[Nginx 为什么不使用多线程？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题3nginx-为什么不使用多线程)<br/>
Nginx采用单线程来异步非阻塞处理请求（管理员可以配置Nginx主进程的工作进程的数量），不会为每个请求分配cpu和内存资源，节省了大量资源，同时也减少了大量的CPU的上下文切换，所以才使得Nginx支持更高的并发。

### 题4：[Nginx 都有哪些应用场景？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题4nginx-都有哪些应用场景)<br/>
**http服务器**

Nginx是一个http服务可以独立提供http服务，也可以做网页静态服务器。

**虚拟主机**

可以实现在一台服务器虚拟出多个网站，例如个人网站使用的虚拟机。

**反向代理，负载均衡**

当网站的访问量达到一定程度后，单台服务器不能满足用户的请求时，需要用多台服务器集群可以使用Nginx作为反向代理。并且多台服务器可以平均分担负载，不会出现某台服务器负载高宕机而某台服务器闲置的情况。

**配置安全管理**

使用Nginx搭建API接口网关，对每个接口服务进行拦截。

### 题5：[Nginx目录结构都有哪些？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题5nginx目录结构都有哪些)<br/>
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

### 题6：[Nginx 中如何在 URL 中保留双斜线？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题6nginx-中如何在-url-中保留双斜线)<br/>
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

### 题7：[Nginx 和 apache 有什么区别？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题7nginx-和-apache-有什么区别)<br/>
Nginx轻量级，同样是web服务，比apache 占用更少的内存及资源；

抗并发，nginx处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能；

高度模块化的设计，编写模块相对简单；最核心的区别在于apache是同步多进程模型，一个连接对应一个进程；

nginx是异步的，多个连接（万级别）可以对应一个进程。

### 题8：[Nginx 中如何限制并发连接数？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题8nginx-中如何限制并发连接数)<br/>
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

### 题9：[Nginx 中 location指令的作用是什么？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题9nginx-中-location指令的作用是什么)<br/>
location指令的作用是根据用户请求的URI来执行不同的应用。

location指令也可以理解成为根据用户请求的网站URL进行匹配，匹配成功即进行相关的操作。

### 题10：[Nginx 中如何解决前端跨域问题？](/docs/Nginx/2021年Nginx面试题大汇总附答案.md#题10nginx-中如何解决前端跨域问题)<br/>
使用Nginx转发请求。

把跨域的接口写成调本域的接口，然后将这些接口转发到真正的请求地址。

### 题11：nginx-中是如何实现高并发<br/>


### 题12：nginx-中常见状态码有哪些<br/>


### 题13：nginx-中产生-502-错误可能原因<br/>


### 题14：什么是-nginx<br/>


### 题15：nginx-服务器解释--s-参数有什么作用<br/>


### 题16：nginx-中如何禁止某ip不可访问<br/>


### 题17：nginx-中什么是-c10k-问题<br/>


### 题18：为什么要使用-nginx<br/>


### 题19：为什么-nginx-性能这么高<br/>


### 题20：nginx-中-stub_status-和-sub_filter-指令有什么作用<br/>


### 题21：为什么-nginx-要做动静分离<br/>


### 题22：nginx-中如何获得当前的时间<br/>


### 题23：nginx-中-master-和-worker-进程分别是什么<br/>


### 题24：ngx_http_upstream_module-模块有什么作用<br/>


### 题25：nginx-中有可能将错误替换为-502503-错误吗<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")