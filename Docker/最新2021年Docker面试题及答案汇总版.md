# 最新2021年Docker面试题及答案汇总版

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[如何批量清理临时镜像文件？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题1如何批量清理临时镜像文件)<br/>
可以使用<code>docker rmi $(docker images -q -f dangling = rue)</code>命令。

### 题2：[Docker 中什么是 Image？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题2docker-中什么是-image)<br/>
image即镜像。image相当于container的模板，container创建后里面有什么软件完全取决于它使用什么image。

image可以通过container创建，相当于把此时container的状态保存成快照，也可以通过Dockerfile来创建。其中通过Dockerfile创建的方法能让环境配置和代码一起被版本库一起管理。

### 题3：[Docker 中仓库、注册服务器、注册索引有什么联系？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题3docker-中仓库注册服务器注册索引有什么联系)<br/>
仓库（Repository）是存放一组关联镜像的集合，比如同一个应用的不同版本的镜像。

注册服务器（Registry）是存放实际的镜像的地方。

注册索引（Index）则负责维护用户的账号、权限、搜索、标签等管理。

注册服务器利用注册索引来实现认证等管理。

### 题4：[非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题4非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”)<br/>
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
```
重启docker服务即可。

### 题5：[如何备份系统中所有的镜像？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题5如何备份系统中所有的镜像)<br/>
备份镜像列表可以使用命令：
```shell
docker images | awk 'NR>l{prin七$1":"$2} ' | sort > images.list
```

导出所有镜像为当前目录下文件， 可以使用如下命令：

```shell
while read img; do 
echo $img 
file="${img/\//-}" 
sudo docker save --output $file. tar $img 
done< images.list
```

将本地镜像文件导入为Docker镜像：

```shell
while read img; do 
echo $img 
file="${img/\//-}" 
docker load< $file.tar 
done< images.list
```

### 题6：[Docker 中本地镜像文件一般存放在什么位置？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题6docker-中本地镜像文件一般存放在什么位置)<br/>
Docker相关的本地资源存在/var/lib/docker/目录下。

其中container目录存放容器信息，graph目录存放镜像信息，aufs目录下存放具体的镜像底层文件。

### 题7：[Docker 需要查询日志应该使用什么命令？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题7docker-需要查询日志应该使用什么命令)<br/>
```shell
docker logs
```

### 题8：[Docker的配置文件放在什么位置，如何修改配置？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题8docker的配置文件放在什么位置如何修改配置)<br/>
Ubuntu系统下Docker的配置文件是/etc/default/docker，CentOS系统配置文件存放在/etc/sysconfig/docker。

### 题9：[容器与主机之间的数据拷贝命令是什么？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题9容器与主机之间的数据拷贝命令是什么)<br/>
<code>docker cp</code>命令，用于容器与主机之间的数据拷贝。
 
1、从主机往容器中拷贝

将主机/www/jingxuan目录拷贝到容器a1234556789的/www目录下。

```shell
docker cp /www/jingxuan a1234556789:/www/
```

2、将容器中文件拷往主机

将容器a1234556789的/www目录拷贝到主机的/tmp目录中。
```shell
docker cp  a1234556789:/www /tmp/
```

### 题10：[Docker 中如何查看输出和日志信息？](/docs/Docker/最新2021年Docker面试题及答案汇总版.md#题10docker-中如何查看输出和日志信息)<br/>
当要查看一个docker容器的日志时，可以直接使用
```shell
docker logs 容器名字或者ID。
```
如果需要找其中包含某些内容（如xxx）的所有行，可以使用
```shell
docker logs 容器名字或者 ID 2>&1 | grep xxx
```
这里的2>&1代表把标准错误（文件描述符2）重定向（>）到标准输出（文件描述符 1）的位置（&）。

如果需要导出日志文件，可以使用
```java
# grep 的 -i 表示不区分大小写 
docker inspect 容器名字或者 ID | grep -i logpath
```

### 题11：docker-有哪些优缺点<br/>


### 题12：如何控制容器占用系统资源cpu内存的份额<br/>


### 题13：解释一下-dockerfile-的-onbuild-指令<br/>


### 题14：docker-如何临时退出正在交互容器终端<br/>


### 题15：容器退出后-通过-docker-ps-命令查看不到数据会丢失么<br/>


### 题16：什么是-docker-容器<br/>


### 题17：dockerfile中-copy-和-add-命令有什么区别<br/>


### 题18：什么是容器什么是-docker<br/>


### 题19：docker-中什么是-dockerfile<br/>


### 题20：docker-中如何批量清理容器和镜像文件<br/>


### 题21：docker-容器和主机之间如何复制数据<br/>


### 题22：docker-镜像和层有什么区别<br/>


### 题23：什么是-docker-镜像<br/>


### 题24：生产环境中如何监控-docker<br/>


### 题25：docker-安全吗-<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")