# 2021年Docker面试题大汇总附答案

### 全部面试题答案，更新日期：01月30日，直接下载吧！

### 下载链接：[高清500+份面试题资料及电子书，累计 10000+ 页大厂面试题  PDF](/docs/index.md)

## Docker

### 题1：[Docker 中如何批量清理容器和镜像文件？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题1docker-中如何批量清理容器和镜像文件)<br/>
1、清理所有已经停止的容器
```shell
docker rm $(docker ps -a -q)
```
2、清理所有镜像
```shell
docker rmi $(docker images -q)
```
3、强制清理所有镜像
```shell
docker rmi -f $(docker images -q)
```
4、清理过滤出来的镜像
```shell
docker rmi $(docker images | grep "关键字" | awk '{print $3}')

### 题2：[非官方仓库下载镜像时，可能提示“Error：Invaild registry endpoint https://dl.docker.com:5000/v1/…”？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题2非官方仓库下载镜像时可能提示“error-invaild-registry-endpoint-https://dl.docker.com:5000/v1/…”)<br/>
非官方地址，例如：dl.dockerpool.com。

Docker自1.3.0版本之后，加强了对镜像安全性的验证，需要手动添加对非官方仓库的信任。 

```shell
DOCKER_OPTS=”–insecure-registry dl.dockerpool.com:5000” 
```
重启docker服务即可。

### 题3：[Docker 中如何查看输出和日志信息？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题3docker-中如何查看输出和日志信息)<br/>
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

### 题4：[如何控制容器占用系统资源（CPU、内存）的份额？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题4如何控制容器占用系统资源cpu内存的份额)<br/>
使用<code>docker [container] create</code>命令创建容器或使用<code>docker [con­tainer] run</code>创建并启动容器的时候，可以使用<code>-c | - cpu -shares[=O]</code>参数来调整容器使用CPU的权重；使用<code>-ml-memory[=MEMORY]</code>参数来调整容器使用内存的大小。


### 题5：[Docker 中什么是 Container？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题5docker-中什么是-container)<br/>
container即容器。可以把每个container看做是一个独立的主机。 

container的创建通常有一个image作为其模板。类比成虚拟机的话可以理解为image就是虚拟机的镜像，而container就是一个个正在运行的虚拟机。一个虚拟机镜像可以创建出多个运行的虚拟主机且相互独立。

注意：container一旦创建如果没有用rm命令移除，将会一直存在，因此在不使用的情况下需要手动删除。

### 题6：[DockerFile中 COPY 和 ADD 命令有什么区别？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题6dockerfile中-copy-和-add-命令有什么区别)<br/>
COPY指令和ADD指令的唯一区别在于是否支持从远程URL获取资源。

COPY指令只能从执行docker build所在的主机上读取资源并复制到镜像中，而ADD指令还支持通过URL从远程服务器读取资源并复制到镜像中。

### 题7：[生产环境中如何监控 Docker？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题7生产环境中如何监控-docker)<br/>
1、Docker提供docker:stats和docker事件等工具来监控生产中的docker。我们可以使用这些命令获取重要统计数据的报告。

2、Docker统计数据：当我们使用容器ID调用docker stats时，我们获得容器的CPU，内存使用情况等。它类似于Linux中的top命令。

3、Docker事件：docker事件是一个命令，用于查看docker守护程序中正在进行的活动流。

一些常见的docker事件：attach、commit、die、detach、rename、destroy等。还可以使用各种选项来限制或过滤我们关注的事情。

### 题8：[Docker 容器中如何启动 Nginx 服务？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题8docker-容器中如何启动-nginx-服务)<br/>
启动nginx服务，端口映射随机，并挂载本地文件目录到容器html的命令。

```shell
docker run -d -P --name nginx2 -v /home/nginx:/usr/share/nginx/html nginx
```

### 题9：[Docker 中如何查看镜像支持环境变量？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题9docker-中如何查看镜像支持环境变量)<br/>
**方式一：docker inspect**

通过docker inspect命令不仅能查看环境变量，还能查看容器其它相关信息，非常丰富，以Json格式输出。
```shell
docker inspect centos
```

可读性还可以，但也不算很高，可以通过grep命令过滤一下：
```shell
$ docker inspect centos | grep SERVER "SERVER_PORT=80"
```

或者可以解析一下Json文本：

```shell
$ docker inspect -f '{{range $index, $value := .Config.Env}}{{println $value}}{{end}}' centosPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

**方式二：doecker exec env**

这种方式获取的环境变量就跟我们平时获取linux环境变量是一样的了。只是在容器跑了个env命令而已。如下：
```shell
$ docker exec centos envPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binHOSTNAME=f8b489603f31HOME=/root
```

### 题10：[Docker 中仓库、注册服务器、注册索引有什么联系？](/docs/Docker/2021年Docker面试题大汇总附答案.md#题10docker-中仓库注册服务器注册索引有什么联系)<br/>
仓库（Repository）是存放一组关联镜像的集合，比如同一个应用的不同版本的镜像。

注册服务器（Registry）是存放实际的镜像的地方。

注册索引（Index）则负责维护用户的账号、权限、搜索、标签等管理。

注册服务器利用注册索引来实现认证等管理。

### 题11：什么是-docker-镜像<br/>


### 题12：开发环境中-docker-与-vagrant-该如何选择<br/>


### 题13：docker-安全吗-<br/>


### 题14：docker-环境如何迁移到另外宿主机<br/>


### 题15：docker-容器如何运行在非linux系统<br/>


### 题16：ci持续集成服务器的功能是什么<br/>


### 题17：docker-中什么是-registry<br/>


### 题18：docker-需要查询日志应该使用什么命令<br/>


### 题19：如何备份系统中所有的镜像<br/>


### 题20：如何批量清理临时镜像文件<br/>


### 题21：docker-中什么是-image<br/>


### 题22：什么是docker-hub<br/>


### 题23：docker-镜像和层有什么区别<br/>


### 题24：docker-中什么是-dockerfile<br/>


### 题25：什么是-docker-容器<br/>


![大厂面试题](../../imgs/pages.jpg "Java精选")

![大厂面试题](../../imgs/pdfs.png "Java精选")

![大厂面试题](../../imgs/weixin.png "Java精选")